docker compose yml 設定檔
-------------------------

從一個範例說起，ghost docker-compose yml。

ghost 已有現成 docker 為什麼需要在建立一個？主要是當你需要客制時就需要有建置的機制

```
build:
  container_name: ghost_build
  image: trunk/ghost_env
  command: "/bin/bash -l -c 'npm i --no-bin-links && npm i sqlite3 --no-bin-links  && git submodule update --remote --merge && grunt init && grunt prod'"
  working_dir: /ghost
  volumes:
    - ./:/ghost

web:
  container_name: ghost_env
  image: trunk/ghost_env
  command: "/bin/bash -l -c 'npm start --production'"
  ports:
    - 2368:2368
  working_dir: /ghost
  volumes:
    - ./:/ghost
  restart: always
```

上述指令說明：

container_name
--------------

container_name 別名，等於 docker --name，一般建議都給個 name 方便讓指令可讀性提高

image or build
--------------

可以使用既有的 image 也可以指定 Dockerfile 進行建置

command
-------

啟動 Container 要執行的指令，可以想像成 docker run web command

working_dir
-----------

下 command 時的所在資料夾位置

ports
-----

你要開放的 port 號，參數對應為 HOST:CONTAINER

已下列例子來說

```
ports:
  - 80:2368
```

指的是把 Container 裡面 2368 的 port 號 mapping 到 host 的 80 port

volumes
-------

類似 virtualbox share folder 的功能，當你需要建置，或是把相關資料保留在 host 時非常好用
