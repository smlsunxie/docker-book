docker-swarm 建立 docker cluster
================================

建立 swarm-master
-----------------

### 建立 swarm token 並取得

#### OSX or Windows

假設在 OSX 中進行 demo 的話，我們可以透過下列指令取得

```
docker-machine create -d virtualbox local
eval "$(docker-machine env local)"
docker run swarm create
```

執行完上述指令，將會得到

`d39465443c94a130608eb76871de1266`

將會作為之後進行 cluster 使用

#### ubuntu

執行 `docker run swarm create` 即可取得

建立 swarm-master 透過 docker-machine
-------------------------------------

使用在上一步驟取得的 swarm token

```
docker-machine create \
-d virtualbox \
--swarm \
--swarm-master \
--swarm-discovery token://d39465443c94a130608eb76871de1266 \
swarm-master
```

建立 swarm-node
---------------

```
docker-machine create \
-d virtualbox \
--swarm \
--swarm-discovery token://d39465443c94a130608eb76871de1266 \
swarm-agent-00

docker-machine create \
-d virtualbox \
--swarm \
--swarm-discovery token://d39465443c94a130608eb76871de1266 \
swarm-agent-01
```

檢查目前已建立的 docker-machine
-------------------------------

`docker-machine ls`

```
NAME             ACTIVE   DRIVER       STATE     URL                         SWARM
default          -        virtualbox   Stopped
local            -        virtualbox   Running   tcp://192.168.99.101:2376
swarm-agent-00   -        virtualbox   Running   tcp://192.168.99.103:2376   swarm-master
swarm-agent-01   -        virtualbox   Running   tcp://192.168.99.104:2376   swarm-master
swarm-master     -        virtualbox   Running   tcp://192.168.99.102:2376   swarm-master (master)
ubuntuvm         -        generic      Stopped   tcp://localhost:2376
```

透過 docker-machine 載入 swarm-master 進行控制
----------------------------------------------

`eval $(docker-machine env --swarm swarm-master)`

列出目前 docker 資訊
--------------------

`docker info`

```
Containers: 4
Images: 3
Role: primary
Strategy: spread
Filters: health, port, dependency, affinity, constraint
Nodes: 3
 swarm-agent-00: 192.168.99.103:2376
  └ Containers: 1
  └ Reserved CPUs: 0 / 1
  └ Reserved Memory: 0 B / 1.022 GiB
  └ Labels: executiondriver=native-0.2, kernelversion=4.0.9-boot2docker, operatingsystem=Boot2Docker 1.8.2 (TCL 6.4); master : aba6192 - Thu Sep 10 20:58:17 UTC 2015, provider=virtualbox, storagedriver=aufs
 swarm-agent-01: 192.168.99.104:2376
  └ Containers: 1
  └ Reserved CPUs: 0 / 1
  └ Reserved Memory: 0 B / 1.022 GiB
  └ Labels: executiondriver=native-0.2, kernelversion=4.0.9-boot2docker, operatingsystem=Boot2Docker 1.8.2 (TCL 6.4); master : aba6192 - Thu Sep 10 20:58:17 UTC 2015, provider=virtualbox, storagedriver=aufs
 swarm-master: 192.168.99.102:2376
  └ Containers: 2
  └ Reserved CPUs: 0 / 1
  └ Reserved Memory: 0 B / 1.022 GiB
  └ Labels: executiondriver=native-0.2, kernelversion=4.0.9-boot2docker, operatingsystem=Boot2Docker 1.8.2 (TCL 6.4); master : aba6192 - Thu Sep 10 20:58:17 UTC 2015, provider=virtualbox, storagedriver=aufs
CPUs: 3
Total Memory: 3.065 GiB
Name: b67a95e151a6
```

運行多個 docker run
-------------------

`docker run hello-world * 4`

根據系統資源分派 docker 運行在不同機器上
----------------------------------------

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS                                     NAMES
da7a4bdd3024        hello-world         "/hello"                 6 seconds ago       Exited (0) 2 seconds ago                                              swarm-agent-00/prickly_almeida
fd9d1466e769        hello-world         "/hello"                 29 seconds ago      Exited (0) 24 seconds ago                                             swarm-master/lonely_torvalds
2fe3b693a9ba        hello-world         "/hello"                 57 seconds ago      Exited (0) 53 seconds ago                                             swarm-agent-01/gloomy_bohr
ae08b358067c        hello-world         "/hello"                 7 minutes ago       Exited (0) 7 minutes ago                                              swarm-agent-01/stoic_kalam
ffe2fee033d4        hello-world         "/hello"                 8 minutes ago       Exited (0) 8 minutes ago                                              swarm-agent-00/nostalgic_swartz
ebf836a25ed2        swarm:latest        "/swarm join --advert"   13 minutes ago      Up 13 minutes               2375/tcp                                  swarm-agent-01/swarm-agent
53f407000e7d        swarm:latest        "/swarm join --advert"   15 minutes ago      Up 15 minutes               2375/tcp                                  swarm-agent-00/swarm-agent
d8051f280e95        swarm:latest        "/swarm join --advert"   18 minutes ago      Up 18 minutes               2375/tcp                                  swarm-master/swarm-agent
b67a95e151a6        swarm:latest        "/swarm manage --tlsv"   18 minutes ago      Up 18 minutes               2375/tcp, 192.168.99.102:3376->3376/tcp   swarm-master/swarm-agent-master

```
