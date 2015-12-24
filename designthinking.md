Design thinking of Online development runtime
====
#Prepare
##Design thinking
###Thinking
- The cloud part should be hidden for developer.

###Feeling
- Too many things need to know for developer before start coding

###Ideas
- TODO

#Initialize online runtime
##Design thinking
###Thinking
- I need to think where to put the VM. 
- Dose the VM could access docker hub fast
- Which OS need to be installed on VM
- Dose the VM could support the rumtime's requirement
- manage the shared folder between git and runtime containers
- manage runtime container with attach resource, such as database, cache

###Feeling
- why I need to care VM? It should be always there for using.
- can I only care the code without think about the hosting.
- can I setup runtime and attach resource in one step

###Feeling
- If I already have the VM, I could create project on VM by one command line
	+ Create project folder on my laptop
	+ Create remote git repo
	+ init git repo, and setup auto push after commit
	+ Add remote git repo to my local git
- Setup the runtime and attach resource by one configuration file.

#Development
##Design thinking
###Thinking
- Dose the code push to remote?
- Dose the code effective?
- How's going of the runtime
- Do I need to run database migration or lint script in runtime or local?

###Feeling
- know what is happening on runtime
- the synchronized still not convenient 
- I can manage the runtime dirctly 

###Ideas
TODO

