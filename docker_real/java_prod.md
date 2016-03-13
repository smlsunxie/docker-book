建立 production mode image
==========================

建立 Dockerfile
---------------

```
FROM agileworks/java7_maven_dockerfile
COPY ./ /app
COPY ~/.m2 /root/.m2
EXPOSE 8000

WORKDIR /app
CMD /bin/bash -c 'cd data-rest && mvn spring-boot:run'
```

執行建置指令
------------

需要注意的是，一旦運行 `docker build`，docker 檔案的存取只能局限於 docker build 運行的所在位置。

所以若有超出範圍的檔案結構，需要事先進行複製，以此為例，我們希望加快建置速度，所以我們要把 `~/.m2` 先行複製到建置專案的根目錄。

```
cp -r ~/.m2/ .m2
docker build -t agileworks/spring_prod .
```

運行 production image
---------------------

```
docker run -p 8000:8000 agileworks/spring_prod
```
