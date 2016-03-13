取得 base Ubuntu image
----------------------

此章節將會透過 Ubuntu 作為相關 Docker Image 製作的基底。

相關步驟如下
------------

-	`docker pull ubuntu:14.04`
-	`docker run -it --name ubuntu_base ubuntu:14.04  /bin/bash -l`

上面指令的解析如下：

-	`--name`: docker 運行時的別名，讓我們可以方便進行操作時的索引
-	`-i`: Keep STDIN open even if not attached
-	`-t`: Allocate a pseudo-tty

一般來說 `-it` 會用於 shell command 互動模式，當我們需要 login container 時會用到。

> For interactive processes (like a shell), you must use -i -t

-	`/bin/bash -l`: 執行的指令
