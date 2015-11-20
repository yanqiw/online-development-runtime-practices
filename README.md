Overview
====
#What's you need to know before read
- git
- docker
- SSH
- Django (Optional, The sample uses django)

#Basic idea
The basic idea of create online development runtime present on [frankwang.cn](http://frankwang.cn)
##Architecture overview
![](http://frankwang.cn/img/Architecture_Overview.png)

#Prepare
##Get a VM on cloud
For running the cloud runtime, I create one VM on cloud.
###Digital ocean
Digital ocean is a good choise to create the VM, as it's very simple.

[Click here to Digital ocean guide](https://cloud.digitalocean.com/support/suggestions?query=How%20do%20I%20create%20a%20Droplet%3F) 

##Install docker on VM
Development runtime is running in a docker container, the docker needs to be installed on the VM.

[Click here to docker installation guide](https://docs.docker.com/engine/installation/ubuntulinux/)

##Install git on laptop
I use git to synchronized between local and online runtime. 

[Click here to download git](https://git-scm.com/downloads)

##Install SSH client on laptop
I need to login my VM by SSH client. Below are two options:
- [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
- [MobaXterm](http://mobaxterm.mobatek.net/)

I prefer MobaXterm, as it provides more powerful tools

#Initialize online runtime

#Development

#Deploy