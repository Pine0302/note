### redis笔记
redis 拼团

拼团人数控制

三人团
提交订单-->支付-->参与拼团

支付的时候可能调取了第三方接口，没法做控制，所以在提交订单的时候做控制

针对三人团，只允许有不超过三个人处于待支付和已支付状态

用户点击提交订单-->检查该团是否满三人-->
1.不满三人，可以提交订单
2.满人数，提醒暂时无法拼团

检查这个动作需要加锁  
加锁方式  redis 原子性

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

