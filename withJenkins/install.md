docker 執行環境安裝
===================

建立 check jenkins environment docker task
------------------------------------------

指令如下

```
id
docker -v
docker-compose -v
docker ps
docker login -e smlsun.xie@gmail.com -p jenkins4ever -u trunkworkshopjenkins
```

範例結果如下

```
uid=116(jenkins) gid=125(jenkins) groups=125(jenkins),998(docker)
Docker version 1.8.3, build f4bf5c7
docker-compose version: 1.4.2
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                    NAMES
a71fb8e3990f        miiixr/picklete_env   "/bin/bash -l -c 'npm"   16 hours ago        Up 2 minutes        0.0.0.0:1337->1337/tcp   nodejsSample
dc968421b7dc        dgraziotin/mysql      "/run.sh"                16 hours ago        Up 2 minutes        0.0.0.0:3306->3306/tcp   mysql
Finished: SUCCESS
```
