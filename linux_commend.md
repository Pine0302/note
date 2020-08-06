### linx——commend
> awk
cat development.log | awk -F 'ydb.DEBUG:' '{print $2}'   隔断输出
awk '/delete—cron/{print}' development.log            or  cat development.log | awk '/delete—cron/{print}'  匹配输出