使用 docker 指令建立 maven image
================================

install maven
-------------

首先 pull java 7 image `docker pull java:7`

```
docker run -it java:7 --name java7 /bin/bash -l
```

maven 安裝指令如下：

```
apt-get update
apt-get install maven
```

登入後執行上述安裝指令。

安裝完成後，將安裝完成的 container commit 為新的 image

```
docker commit java7 agileworks/java7_maven
```

確認一下是否可以正確執行 maven

```
docker run --rm agileworks/java7_maven mvn -v
```

取得 sample 專案
----------------

```
mkdir ~/workspace && cd ~/workspace
git clone https://github.com/agileworks-tw/spring-boot-sample.git
cd spring-boot-sample/
```

使用 docker 運行專案
--------------------

```
docker run --rm \
-v $HOME/.m2:/root/.m2 \
-v `pwd`:/app \
-w /app \
-p 8000:8000 \
agileworks/java7_maven \
/bin/bash -c 'cd data-rest && mvn spring-boot:run'
```
