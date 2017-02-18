---
title: "Docker One-liners"
date: 2016-02-18
---

Let other people care about the installation procedures. The best thing about docker is that you get your app installed and running with just one line of code. I am listing here my favorite docker images and one-liners to run them.

My favorite way to run docker containers is a virtual machine on [Digital Ocean](https://m.do.co/c/673c97887267) with Ubuntu and docker preinstalled (see 'How to start Digital Ocean droplet?' [here](https://github.com/churchill-lab/sysgen2015) ).

### RStudio Server

I use [rocker/hadleyverse](https://hub.docker.com/r/rocker/hadleyverse/) docker image

```
docker run -d -p 8787:8787 -e USER=rstudio -e PASSWORD=your_secret_password rocker/hadleyverse
```

And I also have [my own versions](https://github.com/simecek/GoogleCloud/blob/master/docker/rstudio/Dockerfile) with additional R packages preinstalled:

```
docker run -d -p 8787:8787 -e USER=rstudio -e PASSWORD=your_secret_password simecek/rstudio
```

Even if you forgot to install some software, you can always [log into running docker container](http://simecek.github.io/Log-Into-A-Running-Docker-Container/) and do so.

### Shiny Server

If you do not want to pay for [shinyapps.io](https://www.shinyapps.io/), you need to run your own Shiny Server. Note I use `-v` option to attach `\shiny` folder with my shiny applications.

```docker run -d -p 3838:3838 -v /shiny/:/srv/shiny-server/ -v /srv/shinylog:/var/log rocker/shiny```

### Jupyter Notebook

An easy way to get Jupyter Notebook with Python 2, Python 3 and R.

```
sudo docker run -d -p 80:8888 --restart=always --name dsnb jupyter/datascience-notebook start-notebook.sh --NotebookApp.password='sha1:YOUR*PASSWD*SHA1'
```

Also, Kaggle offers more fancy [IPython notebook](http://jamiehall.cc/post/how-to-get-started-with-data-science-in-containers) with numerous machine learning packages preinstalled

```
docker run -v $PWD:/tmp/working -w=/tmp/working -p 8888:8888 --rm -it kaggle/python jupyter notebook --no-browser --ip="0.0.0.0" --notebook-dir=/tmp/working
```

And who would not like to try Google's deep learning TensorFlow module:

```
docker run -d -p 8888:8888 b.gcr.io/tensorflow/tensorflow sh -c "jupyter notebook /notebook"
```

### Wordpress

Finally, just a few days ago I tried to run my own wordpress for the first time. It actually runs two containers: `wordpress` itself and the linked `mysql` container.

```
docker run --name wordpressdb -e MYSQL_ROOT_PASSWORD=your_secret_password -e MYSQL_DATABASE=wordpress -d mysql
docker run -e WORDPRESS_DB_PASSWORD=your_secret_password -d --name wordpress --link wordpressdb:mysql -p 8080:80 -v "$PWD/wordpress/":/var/www/html  wordpress
```
