# Lab 2 - Your first docker images

## Create a transitive image

### Tips

- Use the `docker commit`

### Make our image

1. Run `nginx` image detached with param `-p 80:80` named `mynginx1`

docker run -d -p 80:80 --name mynginx1 nginx

Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
8820d4aadbcd: Pull complete 
26be9a3603ae: Pull complete 
d8ded761e35b: Pull complete 
ddc6b36d0e36: Pull complete 
9a80f9a05524: Pull complete 
baccdd222209: Pull complete 
8db204c3cb06: Pull complete 
Digest: sha256:33e0bbc7ca9ecf108140af6288c7c9d1ecc77548cbfd3952fd8466a75edefe57
Status: Downloaded newer image for nginx:latest
7b5969362aa8b5b0e636dfe87e826b5fcbedce88213a0a211c86571f8d76ccd8


2. Check that the nginx page (localhost:80)

 curl http://localhost:80


<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

3. Run a shell without stopping the Container

docker exec -it mynginx1 sh


4. Update the file `/usr/share/nginx/html/index.html` in the container

echo "Hello There!" > /usr/share/nginx/html/index.html

5. Check that the nginx page has been updated (localhost:80)

curl http://localhost:80
Hello There!

6. Create a transitive image named `my_awsome_image`

docker commit mynginx1 my_awsome_image

sha256:c912ae197968c39f708550ea7997d566a2491ab1394b525d08784b6f2e552878

7. Run the new image

docker run -d -p 8080:80 --name mynginx2 my_awsome_image

a05f6922344fa427192e54c90e87bcfe5fee3624e804b411baaa0f2c7b84fc96

8. Check that the modifications are still present (localhost:80)

curl http://localhost:8080
Hello There!

9. Check the layer with the `docker history` command

docker history my_awsome_image

IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
c912ae197968   3 minutes ago   nginx -g daemon off;                            81.9kB    
33e0bbc7ca9e   13 days ago     CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
<missing>      13 days ago     STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
<missing>      13 days ago     EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      13 days ago     ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
<missing>      13 days ago     COPY 30-tune-worker-processes.sh /docker-ent…   16.4kB    buildkit.dockerfile.v0
<missing>      13 days ago     COPY 20-envsubst-on-templates.sh /docker-ent…   12.3kB    buildkit.dockerfile.v0
<missing>      13 days ago     COPY 15-local-resolvers.envsh /docker-entryp…   12.3kB    buildkit.dockerfile.v0
<missing>      13 days ago     COPY 10-listen-on-ipv6-by-default.sh /docker…   12.3kB    buildkit.dockerfile.v0
<missing>      13 days ago     COPY docker-entrypoint.sh / # buildkit          8.19kB    buildkit.dockerfile.v0
<missing>      13 days ago     RUN /bin/sh -c set -x     && groupadd --syst…   105MB     buildkit.dockerfile.v0
<missing>      13 days ago     ENV DYNPKG_RELEASE=1~bookworm                   0B        buildkit.dockerfile.v0
<missing>      13 days ago     ENV PKG_RELEASE=1~bookworm                      0B        buildkit.dockerfile.v0
<missing>      13 days ago     ENV NJS_RELEASE=1~bookworm                      0B        buildkit.dockerfile.v0
<missing>      13 days ago     ENV NJS_VERSION=0.9.1                           0B        buildkit.dockerfile.v0
<missing>      13 days ago     ENV NGINX_VERSION=1.29.1                        0B        buildkit.dockerfile.v0
<missing>      13 days ago     LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      2 weeks ago     # debian.sh --arch 'arm64' out/ 'bookworm' '…   108MB     debuerreotype 0.15


### Upload our image

1. Tag your image with the tag `<dockerHubId>/my_awsome_image:1.0`

docker tag my_awsome_image samigrbj/my_awsome_image:1.0


2. List your docker images
   docker images
   1. What do you see ?
   REPOSITORY                 TAG       IMAGE ID       CREATED             SIZE
my_awsome_image            latest    c912ae197968   7 minutes ago       281MB
samigrbj/my_awsome_image   1.0       c912ae197968   7 minutes ago       281MB
samigrbj/mynodejs          1.0       f99bd2628ca9   About an hour ago   18.5MB
mynodejs                   latest    f99bd2628ca9   About an hour ago   18.5MB
nginx                      latest    33e0bbc7ca9e   13 days ago         281MB
n8nio/n8n                  latest    4a475a6fdeb9   3 weeks ago         1.69GB
alpine                     latest    4bcff63911fc   6 weeks ago         13.3MB
postgres                   15        5ab68e212eab   2 months ago        628MB
jupyter/scipy-notebook     latest    fca4bcc9cbd4   22 months ago       5.42GB


3. Use the CLI to log into your docker account

docker login


4. Push your image

docker push samigrbj/my_awsome_image:1.0

The push refers to repository [docker.io/samigrbj/my_awsome_image]
baccdd222209: Mounted from library/nginx 
26be9a3603ae: Mounted from library/nginx 
8db204c3cb06: Mounted from library/nginx 
ddc6b36d0e36: Mounted from library/nginx 
d8ded761e35b: Mounted from library/nginx 
8820d4aadbcd: Mounted from library/nginx 
5802b9c6c507: Pushed 
9a80f9a05524: Mounted from library/nginx 
1.0: digest: sha256:c912ae197968c39f708550ea7997d566a2491ab1394b525d08784b6f2e552878 size: 2037


5. Check that your image is available on `https://hub.docker.com/repository/docker/<dockerHubId>/my_awsome_image/`

https://hub.docker.com/repository/docker/samigrbj/my_awsome_image/

![alt text](<Screenshot 2025-08-27 at 17.39.35.png>)


6. Delete the repository `https://hub.docker.com/repository/docker/<dockerHubId>/my_awsome_image/settings`

https://hub.docker.com/repository/docker/samigrbj/my_awsome_image/settings

![alt text](<Screenshot 2025-08-27 at 17.40.53.png>)

