# Lab 2 - Your first docker images

## Create a transitive image

### Tips

- Use the `docker commit`

### Make our image

1. Run `nginx` image detached with param `-p 80:80` named `mynginx1`
   docker run -d -p 80:80 --name mynginx1 nginx

2. Check that the nginx page (localhost:80)
   docker ps
   CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                 NAMES
   d8ef6941b446   nginx     "/docker-entrypoint.…"   9 seconds ago   Up 8 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp   mynginx1

3. Run a shell without stopping the Container
   docker exec -it mynginx1 /bin/bash

4. Update the file `/usr/share/nginx/html/index.html` in the container
   echo "<h1>My Awesome Nginx Page</h1>" > /usr/share/nginx/html/index.html

5. Check that the nginx page has been updated (localhost:80)
   ![alt text](image.png)

6. Create a transitive image named `my_awsome_image`
docker commit mynginx1 my_awsome_image

7. Run the new image
docker run -d -p 8080:80 my_awsome_image

8. Check that the modifications are still present (localhost:80)
![alt text](image-1.png)

9. Check the layer with the `docker history` command
docker history my_awsome_image
![alt text](image-2.png)

### Upload our image

1. Tag your image with the tag `<dockerHubId>/my_awsome_image:1.0`
 docker tag my_awsome_image AwatefBR/my_awsome_image:1.0

2. List your docker images
   1. What do you see ?
3. Use the CLI to log into your docker account
docker login

4. Push your image
docker push AwatefBR/my_awsome_image:1.0

5. Check that your image is available on `https://hub.docker.com/repository/docker/<dockerHubId>/my_awsome_image/`
6. Delete the repository `https://hub.docker.com/repository/docker/<dockerHubId>/my_awsome_image/settings`