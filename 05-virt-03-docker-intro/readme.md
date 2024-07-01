#### Задание 1

https://hub.docker.com/repository/docker/d051a/custom-nginx/general

#### Задание 2
2.png

#### Задание 3
3.png

Объяснение 1: Контейнер остановился, потому что нажатие Ctrl-C в docker attach приводит к остановке процесса, который был запущен в контейнере. Это основная причина остановки контейнера, так как команда docker attach подключает к основной сессии контейнера.

Объяснение 2: Проблема заключается в том, что мы изменили порт, на котором слушает Nginx, но Docker все еще перенаправляет трафик на старый порт. Это значит, что теперь нужно настроить Docker для перенаправления трафика на новый порт (81).

ЛИСТИНГ:

```
 lesson_03 docker run -d -p 8080:80 --name custom-nginx-t2 nginx_custom
318f7d0bea2c92ee30f8064d9eaa771faa5a88c9770184df51d834c4b57a459a
➜  lesson_03 docker attach custom-nginx-t2

^C2024/07/01 20:26:25 [notice] 1#1: signal 2 (SIGINT) received, exiting
2024/07/01 20:26:25 [notice] 29#29: exiting
2024/07/01 20:26:25 [notice] 30#30: exiting
2024/07/01 20:26:25 [notice] 31#31: exiting
2024/07/01 20:26:25 [notice] 32#32: exiting
2024/07/01 20:26:25 [notice] 31#31: exit
2024/07/01 20:26:25 [notice] 29#29: exit
2024/07/01 20:26:25 [notice] 30#30: exit
2024/07/01 20:26:25 [notice] 32#32: exit
2024/07/01 20:26:25 [notice] 1#1: signal 17 (SIGCHLD) received from 32
2024/07/01 20:26:25 [notice] 1#1: worker process 31 exited with code 0
2024/07/01 20:26:25 [notice] 1#1: worker process 32 exited with code 0
2024/07/01 20:26:25 [notice] 1#1: signal 29 (SIGIO) received
2024/07/01 20:26:25 [notice] 1#1: signal 17 (SIGCHLD) received from 30
2024/07/01 20:26:25 [notice] 1#1: worker process 30 exited with code 0
2024/07/01 20:26:25 [notice] 1#1: signal 29 (SIGIO) received
2024/07/01 20:26:25 [notice] 1#1: signal 17 (SIGCHLD) received from 29
2024/07/01 20:26:25 [notice] 1#1: worker process 29 exited with code 0
2024/07/01 20:26:25 [notice] 1#1: exit
➜  lesson_03 docker ps -a | grep custom-nginx-t2
318f7d0bea2c   nginx_custom                                                                "/docker-entrypoint.…"   About a minute ago   Exited (0) 48 seconds ago                          custom-nginx-t2
➜  lesson_03 docker start custom-nginx-t2

custom-nginx-t2
➜  lesson_03 docker exec -it custom-nginx-t2 /bin/bash

root@318f7d0bea2c:/# apt-get update
apt-get install nano -y
Get:1 http://deb.debian.org/debian bookworm InRelease [151 kB]
Get:2 http://deb.debian.org/debian bookworm-updates InRelease [55.4 kB]
Get:3 http://deb.debian.org/debian-security bookworm-security InRelease [48.0 kB]
Get:4 http://deb.debian.org/debian bookworm/main arm64 Packages [8688 kB]
Get:5 http://deb.debian.org/debian bookworm-updates/main arm64 Packages [13.7 kB]
Get:6 http://deb.debian.org/debian-security bookworm-security/main arm64 Packages [161 kB]
Fetched 9117 kB in 2s (4120 kB/s)
Reading package lists... Done
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libgpm2 libncursesw6
Suggested packages:
  gpm hunspell
The following NEW packages will be installed:
  libgpm2 libncursesw6 nano
0 upgraded, 3 newly installed, 0 to remove and 11 not upgraded.
Need to get 816 kB of archives.
After this operation, 3585 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bookworm/main arm64 libncursesw6 arm64 6.4-4 [122 kB]
Get:2 http://deb.debian.org/debian bookworm/main arm64 nano arm64 7.2-1+deb12u1 [680 kB]
Get:3 http://deb.debian.org/debian bookworm/main arm64 libgpm2 arm64 1.20.7-10+b1 [14.4 kB]
Fetched 816 kB in 0s (2077 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libncursesw6:arm64.
(Reading database ... 7576 files and directories currently installed.)
Preparing to unpack .../libncursesw6_6.4-4_arm64.deb ...
Unpacking libncursesw6:arm64 (6.4-4) ...
Selecting previously unselected package nano.
Preparing to unpack .../nano_7.2-1+deb12u1_arm64.deb ...
Unpacking nano (7.2-1+deb12u1) ...
Selecting previously unselected package libgpm2:arm64.
Preparing to unpack .../libgpm2_1.20.7-10+b1_arm64.deb ...
Unpacking libgpm2:arm64 (1.20.7-10+b1) ...
Setting up libgpm2:arm64 (1.20.7-10+b1) ...
Setting up libncursesw6:arm64 (6.4-4) ...
Setting up nano (7.2-1+deb12u1) ...
update-alternatives: using /bin/nano to provide /usr/bin/editor (editor) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/editor.1.gz because associated file /usr/share/man/man1/nano.1.gz (of link group editor) doesn't exist
update-alternatives: using /bin/nano to provide /usr/bin/pico (pico) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/pico.1.gz because associated file /usr/share/man/man1/nano.1.gz (of link group pico) doesn't exist
Processing triggers for libc-bin (2.36-9+deb12u7) ...
root@318f7d0bea2c:/# nano /etc/nginx/conf.d/default.conf
root@318f7d0bea2c:/# curl http://127.0.0.1:80
<html><head>Hey, Netology</head><body><h1>I will be DevOps Engineer!</h1></body></html>
root@318f7d0bea2c:/# nginx -s reload
2024/07/01 20:28:31 [notice] 178#178: signal process started
root@318f7d0bea2c:/# curl http://127.0.0.1:80
curl: (7) Failed to connect to 127.0.0.1 port 80 after 0 ms: Couldn't connect to server
root@318f7d0bea2c:/# curl http://127.0.0.1:81
<html><head>Hey, Netology</head><body><h1>I will be DevOps Engineer!</h1></body></html>
root@318f7d0bea2c:/# exit
exit
➜  lesson_03 netstat -an | grep 8080

tcp46      0      0  *.8080                 *.*                    LISTEN
➜  lesson_03 docker port custom-nginx-t2
80/tcp -> 0.0.0.0:8080
➜  lesson_03 curl http://127.0.0.1:8080
curl: (52) Empty reply from server
➜  lesson_03 code .
➜  lesson_03 docker stop custom-nginx-t2
custom-nginx-t2
➜  lesson_03 docker run -d --name custom-nginx-t2 -p 8080:81 nginx
docker: Error response from daemon: Conflict. The container name "/custom-nginx-t2" is already in use by container "318f7d0bea2c92ee30f8064d9eaa771faa5a88c9770184df51d834c4b57a459a". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
➜  lesson_03 docker run -d --name custom-nginx-t2-new -p 8080:81 nginx
fd79570bd4982f6e304b594c0aa8d758e199241d00f7d992339eafa5d11832ea
➜  lesson_03 docker rm --force custom-nginx-t2

custom-nginx-t2
➜  lesson_03 docker rm --force custom-nginx-t2-new
```


#### Задание 4
4.png

ЛИСТИНГ:

```
lesson_03 docker run -d --name centos-container -v $(pwd):/data centos:latest sleep infinity

Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
52f9ef134af7: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
ead3203653575bdd03eafd198f81398c84a82df08aca953a01ac0178a7d7db18
➜  lesson_03 docker run -d --name debian-container -v $(pwd):/data debian:latest sleep infinity

Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
1e60a453843e: Pull complete
Digest: sha256:a92ed51e0996d8e9de041ca05ce623d2c491444df6a535a566dabd5cb8336946
Status: Downloaded newer image for debian:latest
c25f284cae5203e2e86c4d564f3552416dacd40cc255df5014e7f25c5534b0a7
➜  lesson_03 docker exec -it centos-container /bin/bash

[root@ead320365357 /]# echo "Hello from CentOS" > /data/centos_file.txt
[root@ead320365357 /]# ls
bash: $'\320\264ls': command not found
[root@ead320365357 /]# exit
exit
➜  lesson_03 echo "Hello from Host" > $(pwd)/host_file.txt

➜  lesson_03 docker exec -it debian-container /bin/bash

root@c25f284cae52:/# ls /data
2.png  ANSWER.md  Dockerfile  centos_file.txt  host_file.txt
root@c25f284cae52:/# cat /data/centos_file.txt
cat /data/host_file.txt
Hello from CentOS
Hello from Host
root@c25f284cae52:/#
```

#### Задание 5
5.png
Суть предупреждения:

Предупреждение будет касаться отсутствующего файла compose.yaml. Docker Compose укажет, что не смог найти файл, но продолжит использовать docker-compose.yaml.