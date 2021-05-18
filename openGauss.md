+ gsql安装：
```
wget https://obs.myhwclouds.com/dws/download/dws_client_8.0.x_redhat_x64.zip --no-check-certificate
unzip dws_client_8.0.x_redhat_x64.zip
source gsql_env.sh
```
+ gsql 启动
```
gsql -d postgres -U gaussdb -W'890302@Tc' -h 172.19.0.2 -p 5432
```

+ docker composer 
```
version: '3.3'

services:

  opengauss:
    restart: always
    image: enmotech/opengauss:2.0.0
    ports:
      - 15432:5432
    environment:
      GS_PASSWORD: 890302@Tc
```