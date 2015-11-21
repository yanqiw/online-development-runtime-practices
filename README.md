Overview
====
#What's you need to know before reading
- git
- docker
- SSH
- Django (Optional, The sample uses django)

#Basic idea
The basic idea of online development runtime presents on [frankwang.cn](http://frankwang.cn)
##Architecture overview
![](http://frankwang.cn/img/Architecture_Overview.png)

#Prepare
##Create a VM on cloud
For running the cloud runtime, I create one VM on cloud.
###Digital ocean
Digital ocean is a good choice to create the VM, as it's very simple.

[Click here to Digital ocean guide](https://cloud.digitalocean.com/support/suggestions?query=How%20do%20I%20create%20a%20Droplet%3F) 

##Install docker on VM
Development runtime is running in a docker container, the docker needs to be installed on the VM.

[Click here to docker installation guide](https://docs.docker.com/engine/installation/ubuntulinux/)

##Install git on laptop
I use git to synchronized code between local and online runtime. 

[Click here to download git](https://git-scm.com/downloads)

##Install SSH client on laptop
I need to login my VM by SSH client. Below are two options:
- [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
- [MobaXterm](http://mobaxterm.mobatek.net/)

I prefer MobaXterm, as it provides more powerful tools

#Initialize online runtime
##Build a bare git repository image and run it
As a way to synchronized my local code to online rumtime is need, I create a bare git repository docker-image which could be accessed by my SSH key.

###Clone [yanqiw/gitsyn](https://github.com/yanqiw/gitsyn) repo
On the VM:
```shell
cd /to/your/workspace
git clone https://github.com/yanqiw/gitsyn
```
####Add your public ssh key
On the VM:
```shell
cd gitsyn
mv sshkeys/authorized_keys.sample sshkeys/authorized_keys 
echo path/your/sshpubilckey > sshkeys/authorized_keys
```
####Build Image
On the VM:
```shell
docker build -t project-name-git-repo .
```
'_project-name_' could be your project name.
####Run the image
On the VM:
```shell
docker run --name project-name-git -v your/host/workdir:/workspace -p YOUR_HOST_PORT:22 project-name-git-repo
```
__YOUR_HOST_PORT__ should not be 22, as the 22 already be used by the host SSH service. 

__your/host/workdir__ should be a folder on the VM, such as /workspace/code

For example: you can use 2233
```shell
docker run --name project-name-git -v your/host/workdir:/workspace -p 2233:22 project-name-git-repo
```

You can refer to [yanqiw/gitsyn](https://github.com/yanqiw/gitsyn) for more details.

##Add remote git repository to local git
On laptop:
```shell
cd /to/project/folder
git remote add runtime ssh://git@YOUR_DOMAIN:YOUR_HOST_PORT/repo/runtime.git
```
- __YOUR_DOMAIN__ is your VM domain, if you don't have a domain for your VM, you can use [ixp.io](http://xip.io) for your domain, or you can use IP of your VM
- __YOUR_HOST_PORT__ is the port on your VM which you open for the git-repo container

##Build development runtime
###Dockerfile
For customized development runtime, I create a Dockerfile for build runtime, and it also be used build deployment package.

####Example
This example builds a django runtime. Create a Dockerfile in the django project, and copy below code in.

Dockerfile
```Dockerfile
FROM python:2.7
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

RUN apt-get update && apt-get install -y \
		mysql-client libmysqlclient-dev \
		postgresql-client libpq-dev \
		sqlite3 \
		gcc \
	--no-install-recommends && rm -rf /var/lib/apt/lists/*

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

Create a 'requirements.txt' file to store the project dependency.

requirements.txt
```text
Django
psycopg2
djangorestframework
markdown
django-filter
mysql-python
``` 

###Push code to VM
As I already initialized the remote git repository, I could push the Dockerfile to VM, and build runtime image on cloud.
```shell
cd /to/project/folder
git commit -am "Add dockerfile"
git push runtime master
```
###Build runtime on VM
On the VM:
```shell
cd /to/project/folder
docker build -t django-rest-runtime .
```

###Run the container
On the VM:
```shell
cd /to/project/folder
docker run --name project-runtime -p 8000:8000 -v "$PWD":/code -d django-rest-runtime
```
__'-v "$PWD":/code'__ is used to attach the code to container.

#Development
##Edit code and commit to online runtime
I can open a IDE, such as sublime text to edit my code. After edited, I commit the code to local git and push to remote. 

###Auto push to remote after commit
I create a git hook to push the code to remote after commit.
 
In your project folder:
```shell
cd .git/hooks
touch post-commit
echo '#!/bin/bash' >> post-commit
echo 'git push runtime master' >> post-commit
```
##Logs
For now, I still need to login the VM to check the logs of runtime

##Debug
TODO, find the way to add break point into runtime

#Deploy
You can choise any deploy tool to manage the deployment. 