# Deploying-the-App-Using-Dockers

## Deploying the app to Dockers

### 1. Create a docker file in root folder. 

### 2. Create a docker image.

    Here with the help of docker file, we create a docker image using the information we write in docker file. That docker image can be taken and run with in a container which will be interacting with the kernel of our OS. Docker is like a container which can be independently run by communicating with the kernel of the OS.

    To create a docker image, we use following commands.
        - FROM
        - COPY
        - WORKDIR
        - RUN
        - EXPOSE
        - CMD

#### 1. FROM Command
    - FROM command is used to select the base image. Docker containers require base image. Base image means we have a Linux OS, on top of that we install other OS.
    
    ```Dockerfile
    FROM python:3.9
    ```

    - When we are building this docker image, it will go and take the baseImage from the docker hub. I.e. it will take a linux based image and on top of that it will install python3.9 and does the other configuration settings.

    - In Short, as soon as docker sees python3.9, from the DockerHub, it takes the baseImage which has Linux on top of it python3.9 and then it does the next step which is COPY.

#### 2. COPY Command

    COPY means whatever files i have in our remote repository, we have to copy them into that baseImage. For that, we copy from current location to a new location in our baseImage. 

    ```Dockerfile
    COPY . /app
    ```
    
    We copy the contents from the current folder to an app folder within our baseImage. The next step is to create that app folder as our working directory.


#### 3. WORKDIR Command

    ```Dockerfile
    WORKDIR /app
    ```

    - We made our app direcotry in baseImage as our working directory.

#### 4. RUN Command

    ```Dockerfile
    RUN pip install -r requirements.txt
    ```

    - Install the packages in requirements.txt file.

#### 5. EXPOSE Command

    ```Dockerfile
    EXPOSE $PORT
    ```

    When docker image run as a container, in order to access the application inside the container, we have to expose some PORT. Then only we can access that entire URL and also from that PORT we are able to access the entire application. So we expose a PORT with in that docker container. $PORT is given because this value when deployed into cloud or server, server is going to automatically assign this port in that container.


#### 6. CMD Command

    1. Run the web application using some commands. 
    
    ```Dockerfile
    CMD gunicorn --workers=4 --bind 0.0.0.0:$PORT app:app
    ```
    > gunicorn is required when we deploy the app into heroku cloud.
    > Here we bind the port number with our local address.


### 3. Configure Github Actions

    > When we want to create github actions using CI/CD pipeline, we create 2 folders,
        1) .github 
        2) workflows

    > We create the above because as soon as we push our code to repository, main.yaml file will have the entire process to do.
        1. Build a docker file.
        2. Push the docker image in the form of a container to the heroku platform.
    
    > So the entire configuration will be set up in the main.yaml file.
    
    > We have to make this a CI/CD pipeline as soon as we push the same to repository. For that, we have add some secret keys in github settings.

        1) Settings > Action Secrets > New Secret
            Set Name and Secret.

    > Create 3 such action secret keys.

![](./images/image1.png)


We set up the Github Actions now, so as soon as workflows folder is seen, all the  files will be deployed.

![](./images/image2.png)

We can see that app is deployed in a container. check right side of the above image.

### Check Github Actions:
https://github.com/jisshub/Boston-House-Price-Prediction/actions
