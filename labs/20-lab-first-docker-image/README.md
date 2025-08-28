# Lab 2 - Your first docker images

## Create a transitive image

### Tips

- Use the `docker commit`

### Make our image

1. Run `nginx` image detached with param `-p 80:80` named `mynginx1`

docker container run --name mynginx1 -d -p 80:80 nginx:latest 


2. Check that the nginx page (localhost:80)


3. Run a shell without stopping the Container

docker exec -it mynginx1 /bin/bash


4. Update the file `/usr/share/nginx/html/index.html` in the container
apt-get install nano
 nano usr/share/nginx/html/index.html 
5. Check that the nginx page has been updated (localhost:80)
6. Create a transitive image named `my_awsome_image`
docker tag nginx:latest my_awesome_image

7. Run the new image

(base) tolo@tolo-Nitro-AN515-52:~/Dev/Cours/Docker/ESGI-4IABD-janvier-2025$ docker container run -td --name my_awesome_image my_awesome_image:latest 
0d562544d4998fbbfa04dc76827fd50a5c4204ba5074ddfeae4ed25b1b6e423f
(base) tolo@tolo-Nitro-AN515-52:~/Dev/Cours/Docker/ESGI-4IABD-janvier-2025$ docker container ps
CONTAINER ID   IMAGE                     COMMAND                  CREATED          STATUS          PORTS                                 NAMES
0d562544d499   my_awesome_image:latest   "/docker-entrypoint.…"   8 seconds ago    Up 7 seconds    80/tcp                                my_awesome_image
8ec7c96d78c8   nginx:latest              "/docker-entrypoint.…"   15 minutes ago   Up 15 minutes   0.0.0.0:80->80/tcp, [::]:80->80/tcp   mynginx1


8. Check that the modifications are still present (localhost:80)



9. Check the layer with the `docker history` command

(base) tolo@tolo-Nitro-AN515-52:~/Dev/Cours/Docker/ESGI-4IABD-janvier-2025$ docker history my_awesome_image:latest 
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
ad5708199ec7   13 days ago   CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
<missing>      13 days ago   STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
<missing>      13 days ago   EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      13 days ago   ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
<missing>      13 days ago   COPY 30-tune-worker-processes.sh /docker-ent…   4.62kB    buildkit.dockerfile.v0
<missing>      13 days ago   COPY 20-envsubst-on-templates.sh /docker-ent…   3.02kB    buildkit.dockerfile.v0
<missing>      13 days ago   COPY 15-local-resolvers.envsh /docker-entryp…   389B      buildkit.dockerfile.v0
<missing>      13 days ago   COPY 10-listen-on-ipv6-by-default.sh /docker…   2.12kB    buildkit.dockerfile.v0
<missing>      13 days ago   COPY docker-entrypoint.sh / # buildkit          1.62kB    buildkit.dockerfile.v0
<missing>      13 days ago   RUN /bin/sh -c set -x     && groupadd --syst…   118MB     buildkit.dockerfile.v0
<missing>      13 days ago   ENV DYNPKG_RELEASE=1~bookworm                   0B        buildkit.dockerfile.v0
<missing>      13 days ago   ENV PKG_RELEASE=1~bookworm                      0B        buildkit.dockerfile.v0
<missing>      13 days ago   ENV NJS_RELEASE=1~bookworm                      0B        buildkit.dockerfile.v0
<missing>      13 days ago   ENV NJS_VERSION=0.9.1                           0B        buildkit.dockerfile.v0
<missing>      13 days ago   ENV NGINX_VERSION=1.29.1                        0B        buildkit.dockerfile.v0
<missing>      13 days ago   LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   # debian.sh --arch 'amd64' out/ 'bookworm' '…   74.8MB    debuerreotype 0.15


### Upload our image

1. Tag your image with the tag `<dockerHubId>/my_awsome_image:1.0`

docker tag my_awesome_image:latest tolo6969/my_awesome_image

2. List your docker images
   1. What do you see ?


   docker images
REPOSITORY                  TAG       IMAGE ID       CREATED          SIZE
mynodejs                    latest    fcc9a456bcd0   54 minutes ago   36.3MB
nodejs                      1.0       fcc9a456bcd0   54 minutes ago   36.3MB
my_awesome_image            latest    ad5708199ec7   13 days ago      192MB
nginx                       latest    ad5708199ec7   13 days ago      192MB
tolo6969/my_awesome_image   latest    ad5708199ec7   13 days ago      192MB
alpine                      3.5       f80194ae2e0c   6 years ago      4MB
couchdb                     2.1       0db0c4109b92   7 years ago      335MB


3. Use the CLI to log into your docker account
4. Push your image
docker push tolo6969/my_awesome_image:latest

5. Check that your image is available on `https://hub.docker.com/repository/docker/<dockerHubId>/my_awsome_image/`
6. Delete the repository `https://hub.docker.com/repository/docker/<dockerHubId>/my_awsome_image/settings`