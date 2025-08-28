# Lab 4 - Dockerfile

## Create your docker image

Create a dockerfile for a Flask application (python).
 
### Create your dockerfile

1. There is two file 
   1. `requirement.txt`, contain python dependencies 
   2. `app.py`, contain our flask app that listen on port `9090`
2. Create a new directory named `myapp` 
3. Copy `requirement.txt` and `app.py` in `myapp`
4. Run `cd myapp`
5. Create a file name `Dockerfile`


* mkdir myapp
* cp app.py requirements.txt myapp/
* cd myapp
* touch Dockerfile



### Modify the dockerfile

1. Use a python image as base
2. Copy `requirement.txt` in `/app/requirements.txt`
3. Define `/app` as working directory
4. Install python dependencies using `pip install -r <file>`
5. Copy `app.py` inside `/app`
6. Specify that the container use the port `9090`
7. Specify the maintainer and the version of the dockerfile
8. Make sure the container will run the command `python app.py`

Dockerfile:
FROM python:3.11-slim

LABEL maintainer="samigrbj" version="1.0"

WORKDIR /app

COPY requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py /app/

EXPOSE 9090

CMD ["python", "app.py"]


### Build the image

1. Build the docker image and name it samigrbj/my_flask:1.0

* docker build -t samigrbj/my_flask:1.0 .

2. Push it to the docker hub


* docker login
* docker push samigrbj/my_flask:1.0

The push refers to repository [docker.io/samigrbj/my_flask]
3cd5add3ba33: Pushed 
9a6263cdeaa5: Pushed 
49d7fdf05d7c: Pushed 
f69de25844d4: Pushed 
a19c392c00af: Pushed 
c682105f5a15: Pushed 
355943c14cb4: Pushed 
811db81590e0: Pushed 
48771443251f: Pushed 
1.0: digest: sha256:bd080e32ca367a05fd36723d9536e5751d9729244dd8a5fcfae7e7ae0fb7d00e size: 856


### Run it 

1. Run your application as `app`

* docker run -d --name app -p 9090:9090 samigrbj/my_flask:1.0

fe35ac37fbacb0d2eb1abf95f09419b9d9b20cd21a0b0796a5fc845a19450af2

2. curl localhost:9090

-> This is a sfeir school about Docker !

![alt text](<Screenshot 2025-08-28 at 12.04.15.png>)
![alt text](<Screenshot 2025-08-28 at 12.10.30.png>)