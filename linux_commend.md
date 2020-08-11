### linx——commend
+ awk
```
cat development.log | awk -F 'ydb.DEBUG:' '{print $2}'   隔断输出
```
```
awk '/delete—cron/{print}' development.log   //  cat development.log | awk '/delete—cron/{print}'  匹配输出
```
+ 当前目录的文件夹和子文件大小
```
du -ah --max-depth=1 
```