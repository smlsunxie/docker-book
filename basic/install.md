安裝 docker
===========

ubuntu
------

```
sudo apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
sudo add-apt-repository "deb https://apt.dockerproject.org/repo ubuntu-$(lsb_release -s -c) main"
sudo apt-get update
sudo apt-get install docker-engine

```

### 設定 user 可以執行 docker

```
sudo usermod -a -G docker {username}
sudo service docker restart
```

### 確認安裝是否完成

```
docker -v
docker ps
docker run hello-world
```

OSX
---

docker-tools: https://www.docker.com/toolbox

### 透過 docker-machine 進行連接

#### 啟動 docker-machine

```
docker-machine start default
```

正常結果為：

```
Starting VM...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
```

就如啟動成功後的訊息，我們可以在執行下列指令：

```
docker-machine env default
```

結果如下：

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/spooky/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
# Run this command to configure your shell:
# eval "$(docker-machine env default)"
```

若要啟動自行載入，可以把

```
$(docker-machine env default)
```

加入 `.bashrc`

又或者自行執行上述指令即可

若要確認是否正常運行，可以執行下列指令：

```
docker ps
```

正常將出現

```
CONTAINER ID  IMAGE  COMMAND  CREATED  STATUS  PORTS  NAMES
```

若出現下列訊息

```
Get http:///var/run/docker.sock/v1.20/containers/json: dial unix /var/run/docker.sock: no such file or directory.
* Are you trying to connect to a TLS-enabled daemon without TLS?
* Is your docker daemon up and running?
```

就有可能需要執行 `$(docker-machine env default)` 來進行連接。

若再不行，請

1.	確定 Virtual Box 是否最新 ，5.0.x 以上
2.	刪除 Virtual Box 管理介面的 default VM
3.	重新執行 Docker Quickstart Terminal
