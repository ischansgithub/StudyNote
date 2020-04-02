## docker的基本操作：

```shell
创建
$nvidia-docker run -ti --name chenww -p 10101:13 --shm-size 4.0G -v /home-ex/tclhk/:/home-ex/tclhk/  --privileged=true  cheney0813/mmdetection:latest /bin/bash

开启
$docker start chenwwnvidia-docker exec -it chenww bash -v /etc/localtime:/etc/localtime

docker里面查看PID
$ps aux | grep pythonkill -9  ${PID}
```

## docker修改容器的挂载目录

```shell
$ docker ps  -a
CONTAINER ID        IMAGE                 COMMAND                  CREATED              STATUS                          PORTS               NAMES
   5a3422adeead        ubuntu:14.04          "/bin/bash"              About a minute ago   Exited (0) About a minute ago                       agitated_newton

$ docker commit 5a3422adeead newimagename

$ docker run -ti -v  "$PWD/dir1":/dir1   -v "$PWD/dir2":/dir2   newimagename  /bin/bash

```

其中-v参数中，冒号":"前面的目录是宿主机目录，后面的目录是容器内目录。