ghost 建置使用 mysql
====================

repository
----------

https://github.com/trunkWorkshop/Ghost`

建置專案 images
---------------

### Dockerfile

`https://github.com/smlsunxie/dockers/blob/master/ghost_env/Dockerfile`

### 建置 image

`docker build -t {yourId}/ghost_env .`

設置 ghost 使用 mysql
---------------------

### 確認 docker environment

`docker-compose run --rm mysql env`

確認在 docker 運行時可以使用的 env

### 設置專案 config

複製 default config

`cp config.example.js config.js`

調整 database 相關設定

```
database: {
  client: 'mysql',
  connection: {
    'user': process.env.GHOST_MYSQL_ENV_MYSQL_USER_NAME || "root",
    'password': process.env.GHOST_MYSQL_ENV_MYSQL_USER_PASS || "root",
    'host': process.env.GHOST_MYSQL_PORT_3306_TCP_ADDR || "127.0.0.1",
    'port': process.env.GHOST_MYSQL_PORT_3306_TCP_PORT || 3306,
    'database': process.env.GHOST_MYSQL_ENV_MYSQL_USER_DB || 'ghost_db',
    charset  : 'utf8'
  }
}
```

### 設置 docker-compose.yml

#### 加入 mysql container

https://github.com/trunkWorkshop/Ghost/blob/master/docker-compose.yml

```
mysql:
  container_name: ghost_mysql
  image: dgraziotin/mysql
  volumes:
    - ../ghosta_database:/var/lib/mysql/

  ports:
    - "3306:3306"

  environment:
    MYSQL_ADMIN_PASS: "root"
    MYSQL_USER_NAME: "ghost"
    MYSQL_USER_DB: "ghost"
    MYSQL_USER_PASS: "ghost"
    CREATE_MYSQL_BASIC_USER_AND_DB: "true"

  restart: always
```

#### 使用 links 讓 web 連結 mysql

```
web:
  container_name: ghost_env
  image: trunk/ghost_env
  command: "/bin/bash -l -c 'npm start --production'"
  ports:
    - 2368:2368
  working_dir: /ghost
  volumes:
    - ./:/ghost
  links:
    - mysql

  restart: always

```

建置專案
--------

`docker-compose run --rm build`

建置專案 debug
--------------

`docker-compose run --rm build /bin/bash -l`

運行專案
--------

`docker-compose up -d web`

運行專案 debug
--------------

新版 docker 提供了 `--service-ports` 讓你可以在 docker 內操作並且令外部可以存取 server

`docker-compose run --rm --service-ports web /bin/bash -l`
