---
date: 2017-02-17T23:19:03-05:00
title: "Data Science Amazon VM With Start/Stop Functionality"
---

Jeff Leek has tried to move to the cloud with his [Chromebook Experiment](http://simplystatistics.org/2016/11/08/chromebook-part2/).

My motivation is different but my goal is similar. Would it be possible to create a virtual machine (VM) in the cloud that after an initial setting...

- It can be started/stopped from the web browser (you do not need SSH into it). Ideally, I also want a command line client to start/stop VM.

- It has RStudio / Jupyter Notebook that starts/stops with the machine. Ideally, R/python updates should be super-easy.

I do not claim that the following solution is necessarily the best one but it works for me. If you are familiar with Amazon EC2:

* Launch a new instance - choose “Ubuntu Server 14.04 LTS (HVM), SSD Volume Type” (because of systemd process manager)
* Be careful, your VM will need public IP, more than default (8 GB) disk space and port 80 open for incoming/outgoing connection
* SSH into it and run the following script

```sh
# run updates
sudo apt-get update
# install docker
sudo apt-get install -y docker.io
# pull docker image jupyter/datascience-notebook (scikit-learn, pandas, … preinstalled)
sudo docker pull jupyter/datascience-notebook
# get YOUR PASSWORD sha1 hash from IPython.lib.passwd(YOUR PASSWORD) 
# start the docker container 
sudo docker run -d -p 80:8888 --restart=always --name dsnb jupyter/datascience-notebook start-notebook.sh --NotebookApp.password='sha1:YOUR PASSWORD SHA1 HASH'
# navigate your browser to http://YOUR_MACHINE_PUBLIC_IP, passwd = YOUR PASSWORD
# make sure everything is running as expected
sudo docker ps
# if not stop and remove the docker container: sudo docker stop dsnb; sudo docker rm dsnb
```

This way I pay for the VM only when I actually use it (plus some pennies for permanent disk space). In February 2017, the cost was $0.05/hour for t2.medium (4GB memory, 2vCPUs) and $0.10/hour for t2.large (8GB memory, 2vCPUs).