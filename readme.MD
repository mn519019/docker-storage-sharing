## Docker Container Volume
- when docker is started, the container can have read-only and read-write access.
- newly inserted data will be stored in a read-write portion.
- It is highly recommended to use overlay2 rather than overlay since overlay2 has more efficient node .utilization

## How to store data permanently 
- $ docker run -d -v /webdata:/var/www/html:ro httpd:latest (Read only).

## How to share data between containers 
- We can create a shared host (think as a mounting point). 
- The host will have a dedicated volume storage that can be shared with other containers.

## How it works? 
- The first conatiner (df) will push the "df -h /" info to the volume's web document (index.html).
- The second container (webserver) will show the contents from the storage and display on the webserver. 
- Think as a very simple Micro Service architecture in a nutshell.

## Instructions
```
## Build Docker files for both containers 
$ docker build -t mn519019/webserver:latest .
$ docker build -t mn519019/df:latest . 

## Check the created images from the build process
$ docker images | grep -i mn519019
mn519019/df          latest    b5d0325cab8c   54 minutes ago   63.1MB < ------
mn519019/httpd       latest    73c10eb9266e   7 days ago       145MB  < ------

## Run containers using -v option 
## The web server should only have a "read-only" option to minimize security breaches
$ docker run -d --name web -p 80:80 -v /webdata:/var/www/html:ro mn519019/httpd
$ docker run -d -v /webdata:/webdata --name df mn519019/df:latest

```

## Outputs 
```
# Expected output of the running containers 
$ docker ps 
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                               NAMES
2c29f42845e3   mn519019/df:latest   "/bin/df.sh"             36 minutes ago   Up 36 minutes                                       df
8ea10e8ba4de   nginx:1.14           "nginx -g 'daemon of…"   47 minutes ago   Up 47 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   web
```

```
# Expected output of the webserver 
$ curl 4.204.222.84
Filesystem      Size  Used Avail Use% Mounted on
overlay          29G  4.9G   25G  17% /
```

## Referecne
- Please look for an "Architecture.png" for better understanding.
- https://www.youtube.com/@ttabae-learn4274
