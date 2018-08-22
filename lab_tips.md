# Tips for lab

You don't need to inject new nodes into Docker containers. You can create the new package in the host machine (the VM) and run it directly from the VM(same for object detector node).

## How to save the changes you've made in the Docker container
You can save the changes you've made to the files by using
```
docker commit CONTAINER_NAME IMAGE:TAG
```
You can see what containers are currently running (and their names) with
```
docker ps
```
For example
```
docker commit happy_student stage:mynewstate
```

## How to "inject" your own code into Docker containers
First we need to share a host volume (a VM directory) with the docker container. For example
```
docker run -v ~/shared:/shared -it --rm stage bash
```
After running that command the directory `shared` in the user's home directory (in this case `/home/user/`) is shared into the docker container.
The files placed into `/home/user/shared` will appear in `/shared` inside the docker container (and vice versa).

Now you can put the files you want to inject into the container inside this shared directory. Copy them to the desired place inside the container and then save the container state (with the `docker commit` command).

## Repositories used in the lab

* [https://github.com/GertKanter/robots_stage](https://github.com/GertKanter/robots_stage) - Stage simulator files/config
* [https://github.com/GertKanter/autosys-object-detector](https://github.com/GertKanter/autosys-object-detector) - The skeleton for object detector node
* [https://github.com/GertKanter/testit_dtron](https://github.com/GertKanter/testit_dtron) - TestIt DTRON extension (the UPPAAL adapter)
