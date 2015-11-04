刪除所有 docker 相關元件
========================

停止所有執行中 docker
---------------------

`docker stop $(docker ps -a -q)`

刪除所有已建立的 containers
---------------------------

`docker rm $(docker ps -a -q)`

刪除所有的 images
-----------------

`docker rmi $(docker images -q)`
