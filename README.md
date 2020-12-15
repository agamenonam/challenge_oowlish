# Oowlish Challenge

There is a sample of this application running on http://18.220.170.250:8000, and if necessary to check the application and token running, i recommend a postman or a insomnia using the token (SomeSecretToken).

The challenge number 1 was solved using the following tools:

* aws ec2 - free tier with ubuntu 20
* python 3 and tools (flask and flask_httpauth)
* docker + docker composer ubuntu installation 
 links [https://docs.docker.com/engine/install/ubuntu/] - [https://docs.docker.com/compose/install/]
* A dockerfile to manage the creation of image to be executed on VM on amazon.
> For the purpose of this challenge is necessary a minimum knowledge of virtualization, linux basic commands using shell, and docker to execute the dockerfile on APP folder and build the image to test the environment.

The docker file below are in descriptive form and are ready to be pasted on a standalone file to be run. And this same file are located on folder *part-1*, submitted on email of the challenge.

```sh
#Please paste this file on APP folder to make it work.
#The most common choice for work with python. Offers common support of python applications versions on 3.x.x
FROM python:3-alpine
#building a environment for python and container to support more apps if needed.
RUN apk add --virtual .build-dependencies \
            --no-cache \
            python3-dev \
            build-base \
            linux-headers \
            pcre-dev
#lib for some c++ and c# applications if needed.
RUN apk add --no-cache pcre
#declare the main folder to work
WORKDIR /app
#copy the files inside de app folder of this challenge to /app folder on container
COPY . /app
#remove the cache files and installators files that is not needed anymore
RUN apk del .build-dependencies && rm -rf /var/cache/apk/*
#in order to avoid some issues running the "requirements.txt", inserted manually the dependencies of challenge using the 2 commands bellow.
RUN pip install flask
RUN pip install flask_httpauth
#declares and expose the 8000 port for recieve and respond orders.
EXPOSE 8000
#specify the python command to not be overrride by parameter. 
ENTRYPOINT ["python"]
#Specify the app.py, in order to declare the application "executable"
CMD ["app.py"]
#declaring the secret as a environment variable to simplify the test, but offering a low level security.
ENV APP_SECRET_TOKEN=SomeSecretToken
#thanks for evalutation and patience in this time.
```

to execute this dockerfile please use the following command, on the same folder of _dockerfile_ file:
```sh
$ docker build -t oowlish-ygor .
```

wait for the steps been executed until the end. In this case there are 15 steps to build the image correctly. And after all steps been executed, you shall see a message like this:
```sh
$ Successfully built {some number}
$ Successfully tagged oowlish-ygor:latest
```

and this is a confirmation that all steps where executed and a image with the name _oowlish-ygor_ was created, and are ready to be executed.
 
 after the step above, run this command to execute the image created:
 
 ```sh
 $ docker run -d -p 8000:8000 oowlish-ygor
 ```
 
 this command will execute the image with the app, on daemon mode. In this mode, the container will be executed, and the shell will be able to execute other functions if required, allowing another microservice be executed.
 
 you can check if the container and application are fully functional with the following command:
 
 ```sh
 $ curl -H "Authorization: Token SomeSecretToken" http://localhost:8000/
 ```
 and the response may be:
 
 ```sh
 $ "message": "DevOps test server is flying!!"
 ```
 
# Part 2

![diagram](http://18.220.170.250/diagram.png)


* This cloud example shows a hosted www.example.com distributed on a proxy environment using cloudfront, to provide a faster response to end-user by geo-location. 
* the cloudfront gets the statics files as images, icons and other files from S3 Bucket, and opens a communication with Api Gateway to recieve requests. 
* The Api Gateway retrieves and load database requests from RDS using Lambda in order to have a high-performance and scalable infra-structure, both of them hosted on a Virtual Private Cloud that allows a fully personalized environment like a on-premisse server. 
