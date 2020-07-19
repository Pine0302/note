### macOS系统使用
> 1.mysql 初始化 timestamp，提示 Invalid default value for 'xxx'
+ 去掉 sql_mode 中的 values: NO_ZERO_IN_DATE,NO_ZERO_DATE 即可
```
show variables like 'sql_mode';
set session sql_mode='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```
