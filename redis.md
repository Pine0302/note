### redis笔记
#### 案例：拼团人数控制

三人团
提交订单-->支付-->参与拼团

支付的时候可能调取了第三方接口，没法做控制，所以在提交订单的时候做控制

针对三人团，只允许有不超过三个人处于待支付和已支付状态

用户点击提交订单-->检查该团是否满三人-->
1.不满三人，可以提交订单
2.满人数，提醒暂时无法拼团

检查这个动作需要加锁  
加锁方式  redis 原子性

乐观锁 watch 
```
$lockValue = uniqid() . rand(10000, 99999);
if (isset($_GPC['teamid']) && (!empty($_GPC['teamid']))) {
    //在插入订单之前确认下该团是否满员
    //锁定确认数据
    $orderConfirmLockResult = $this->memoryCacheService->getOrderConfirmLockResult($lockValue, $teamid);
    if (!$orderConfirmLockResult) {
        $this->message('该团已有用户正在拼团支付，请发起新团或稍后再试！');
        return;
    }
    $result = $this->memoryCacheService->checkCanGroup($teamid);
    if (!$result) {
        $this->message('该团已有用户正在拼团支付，请发起新团或稍后再试！');
        return;
    }
}
```

```
public function getOrderConfirmLockResult($lockValue, $teamid)
{
    $lockResult = false;
    $cacheKeyManage = new CacheKeyManageUtil();
    $redisUtil = new RedisUtil();
    $orderConfirmLockKey = $cacheKeyManage->getOrderConfirmLockResultKey($teamid);
    $lockResult = $redisUtil->singleLock($orderConfirmLockKey, $lockValue, GROUPSPRIZE_ORDER_CONFIRM_LOCK_TTL, GROUPSPRIZE_ORDER_CONFIRM_TRY_TIMES);
    return $lockResult;
}
```

```
 /**
     * 单点加锁
     * @param $key
     * @param $value
     * @param $ttl
     * @param int $tryTimes
     * @return bool   false:加锁失败 true:加锁成功
     */
    public function singleLock($key, $value, $ttl, $tryTimes = 10)
    {
        $redis = redis();
        $tryTime = 0;
        while ($tryTime < $tryTimes) {
            $result = $redis->set($key, $value, ['nx', 'px' => $ttl]);
            if (!empty($result)) {
                return $result;
            } else {
                $tryTime++;
                usleep(500000);//休息0.5秒
            }
        }
        return false;
    }
```
```
 $this->memoryCacheService->unLockOrderConfirm($lockValue, $teamid);
```

```

    /**
     * Set the string value in argument as value of the key.
     *
     * @since If you're using Redis >= 2.6.12, you can pass extended options as explained in example
     *
     * @param string       $key
     * @param string|mixed $value string if not used serializer
     * @param int|array    $timeout [optional] Calling setex() is preferred if you want a timeout.<br>
     * Since 2.6.12 it also supports different flags inside an array. Example ['NX', 'EX' => 60]<br>
     *  - EX seconds -- Set the specified expire time, in seconds.<br>
     *  - PX milliseconds -- Set the specified expire time, in milliseconds.<br>
     *  - PX milliseconds -- Set the specified expire time, in milliseconds.<br>
     *  - NX -- Only set the key if it does not already exist.<br>
     *  - XX -- Only set the key if it already exist.<br>
     * <pre>
     * // Simple key -> value set
     * $redis->set('key', 'value');
     *
     * // Will redirect, and actually make an SETEX call
     * $redis->set('key','value', 10);
     *
     * // Will set the key, if it doesn't exist, with a ttl of 10 seconds
     * $redis->set('key', 'value', ['nx', 'ex' => 10]);
     *
     * // Will set a key, if it does exist, with a ttl of 1000 milliseconds
     * $redis->set('key', 'value', ['xx', 'px' => 1000]);
     * </pre>
     *
     * @return bool TRUE if the command is successful
     *
     * @link     https://redis.io/commands/set
     */
    public function set($key, $value, $timeout = null)
    {
    }
```


> Cache Aside Pattern
+ 失效：应用程序先从cache取数据，没有得到，则从数据库中取数据，成功后，放到缓存中。
+ 命中：应用程序从cache中取数据，取到后返回。
+ 更新：先把数据存到数据库中，成功后，再让缓存失效。


+ 问题1：更新的时候，先删除缓存还是先更新数据库



+ 问题2：更新的时候，让缓存失效好还是更新缓存更好 ? 
  
+ 问题3：先更新数据库，再删除缓存这种方式，会有类似的不一致问题吗


如果A 线程做数据更新，B线程做数据读取，A先删除了缓存成功，在A更新数据库成功之前，B读取数据，走失效过程，把旧数据更新到缓存中，这个时候A更新完数据库 ，则数据库跟缓存数据不一致

如果A,B两个线程同时做数据更新，A先更新了数据库，B后更新数据库，则此时数据库里存的是B的数据。而更新缓存的时候，是B先更新了缓存，而A后更新了缓存，则缓存里是A的数据。这样缓存和数据库的数据也不一致。

发生问题的一个方式，A B 线程 A做更新，B做读取，A







Read/Write Through Pattern


Write Behind Caching Pattern

+ https://coolshell.cn/articles/17416.html 缓存更新的套路
