run 跟 up 的差異
================

run
---

使用 run 時，預設將沒辦法讓共享的 port 可以被 expose 出去

也就是說假若妳用 run 啟動了一個 web 將沒有辦法讓啟動完成的 docker 可以被外部連結

但使用 docker compose 用於在 jenkins 自動化測試時，在 docker 內運行的錯誤將可以被正確回傳 exit code

如此一來，就可以正確反應 test 狀態。

不過運行 run 不能 expose 的問題已被解決，目前可以使用：

1.	希望開啟特定 service 的 port 但不想被掛到 host: service-ports

	EX: docker-compose run --service-ports mysql web

2.	要將特定 port 掛到 host 可以使用： --publish or -p

	docker-compose run --publish 1337:1337 web

up
--

基本上就會將在 docker compose 中有開啟的 port 還有掛載到 host 的 port，

以及對應 links 相關的 Containers 一次啟動完成。
