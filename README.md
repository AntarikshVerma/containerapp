
# Containerize your first dotnet core application
This application will demonstarte how to containerize a dot net core web application using docker

# What and Why Containers- 

It is common scenario when developer code is working fine on his machine however It is not working  after deployment of the same code on different machine. Here containers are come into picture. 
You can visualize container by following example to understand it in better way.
In earlier days It was very difficult to transport the different goods from one place to another. Transportation and packaging was totally dependent on the things which you are going to transport. For example some goods could not be transported through the sea route or via road. To solve this problem invention of container was breakthrough. Container can be packaged  any type of material and product and respective environment for the product can be provided or created in container itself. So It became very easy to transport any type of product through any type of media. 
In the same way, Containers encapsulate code and all its dependencies so the application runs quickly and reliably from one  environment to another environment without hassle of environment related dependency.
There are multiple vendor available which provide containers and Docker is the most popular. In this example we are going to use Docker.

# Steps for creating and running the dotnet core app in container

    1.	Create a web  application  named “containerapp” using below command 

    <b> “dotnet new webapp -o containerapp”</b>

    2.	Create a file in the project root directory “Dockerfile”.If you are using visual studio you can right click on the project and Add new item and add Docker support. It          will automatically create a docker file for you.


   3.	Dockerfile contain all the dependencies and information to run the application. Below the content of docker file 

              #This is base image of dotnet core application which is created by Microsoft.
              FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env
              WORKDIR /app

              #Copy csproj and restore its dependencies such as NuGet packages 
              COPY *.csproj ./
              RUN dotnet restore

              #Copy complete project files and build
              COPY . ./
              RUN dotnet publish -c Release -o out

              #Build runtime image
              FROM mcr.microsoft.com/dotnet/core/aspnet:2.2
              WORKDIR /app
              COPY --from=build-env /app/out .
              ENTRYPOINT ["dotnet", "containerapp.dll", "http://*:5000"]

  4.	Build image and run container – 

      a.	Command to create an image  - container app is name of image and . is for docker file directory location , in this case It is same directory.  This command will              pull all th e required images  mentioned in docker file
         <b> “docker build -t containerapp . ”/<b>
      b.	After successful execution of above command you can check your image by using command <b> “docker images”</b> It will list all the docker images.
      c.	Once the image is successfully created , You can run container by following command 
       “docker run -d -p 4300:80 --name demo_container  containerapp”
          •	-d means to run the container in background
          •	-p means map  the container port like external port: internal port
          •	--name is the container i.e. demo_container
          •	Last argument is image name

      d.	“docker ps” command is used to list out all running containers. 

  5.	If you want to stop or remove your containers and remove images , Below are the commands to do that
      a.	“docker stop containername” – stop container 
      b.	“docker rm containername” – remove container
      c.	“docker rmi imagename” –remove image

 

