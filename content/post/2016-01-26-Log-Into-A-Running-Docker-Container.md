---
title: "Log Into A Running Docker Container"
date: 2016-01-26
---

Imagine your container is running but you would like to "ssh inside". For example your container is R/Shiny server and you need to install a new R package. Or your container is Jupyter Notebook and your forgot the password to access it (it is stored in the environment variable $PASSWORD).

You need a way to log into the container. And the following command is doing exactly that:

```
docker exec -i -t $CONTAINERID /bin/bash
```

where `$CONTAINERID` is a hexadecimal number you get from `docker ps` listing, e.g. `1987416184d8` or `67a0a9d82112` in the example below

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                              NAMES
1987416184d8        churchill/doqtl     "/usr/bin/supervisord"   12 days ago         Up 12 days          1410/tcp, 0.0.0.0:8888->8787/tcp   sleep
67a0a9d82112        rocker/shiny        "/usr/bin/shiny-serve"   5 weeks ago         Up 5 weeks          0.0.0.0:80->3838/tcp               webapp
```