---
title: "Stop And Remove All Containers"
date: 2016-01-30
---

If you want to stop all your running docker containers, use

```
docker stop $(docker ps -a -q)
```

Stopped containers can be restarted with `docker start` command or they can be removed permanently with 

```
docker rm $(docker ps -a -q)
```