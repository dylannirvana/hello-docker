This project is based on the official Docker tutorial https://docs.docker.com/get-started/overview/ using part of the getting-started repo https://github.com/docker/getting-started 

I have found that and other documentation on Docker, inaccurate and cumbersome, and have assembled this Book of Spells, which I feel is a better narrative for learning Docker.

# Docker Book of Spells
Docker may be a bit difficult to understand. I have had to sift through documentation containing a host of inaccuracies (and irrelevant trivia) to arrive at notes that would be helpful to us at BMI. 

I hesitate to use the term tutorial, or even exercise. This is - or will be - the ultimate cheat sheet for properly using Docker. Therefore, it is more like a book of spells, due to its brevity and preciseness. 

### Assumptions
Basic programming, Terminal, Linux commands

### Prerequisites
Install Docker Desktop. You can see what is going on with your images, containers, and volumes via this GUI

Register with Docker Hub to ship images with container > image workflow which we'll see later

## Image > container workflow

### Clone working application
`git clone git@github.com:dylannirvana/hello-docker.git`  
`cd hello-docker`

### Cat Dockerfile
The Dockerfile is the recipe for the particular image you are creating. Using Linux, create and populate Dockerfile  
`cat > Dockerfile`  
`FROM node:alpine`  
`WORKDIR /app`  
`COPY . /app`  
`RUN npm i`  
`CMD ["node", "app.js"]`  
ctr-D to escape

NOTE: To view contents of a file and see you actually wrote to it using cat command, _`cat filename`_ 

By far, _from_ is the most important piece of information here. You create an image based on a particular Linux build (containing Node) from Docker Hub. You create a working directory, copy everything to it, run an npm install, and write a run command called "node".

NOTE: To view application in code editor, type _`code .`_ (I had to configure this behavior in Visual Studio, but it is a fairly common syntax)

### Build an image 
To build a Docker image, you must specify the tag (name) you want to give it using the _`-t`_ flag, and a period or `.` for the current directory you want to pass into it as the sole argument. (Don't forget the dot ".")

`docker build -t hello-docker . `  

You have created an image. Have a look! _`docker images`_ Look at Docker Desktop too. 

### Run a container 
To run a container (also called a process) based on the image you just built (which itself was based on what you declared with "from" in your Dockerfile), you must _map the ports_ from your machine to the container. DON'T just type _`docker run hello-docker`_ because you will have no way to talk to it. The pattern to map the ports is _machine:container_

`docker run -dp 3000:3000 hello-docker`

The _`-d`_ flag is _detached_, meaning it runs in the background. _`-p`_ says you are assigning ports, your way in and out. 

Go to http://localhost:3000/ and you can see it running! 

See the running container (process, hence "ps")
`docker ps`

NOTE: To see all containers, _`docker ps -a`_

### Updating the image
To update the image, you are going to have to ditch the containers running on it. 

Copy the container ID from _`docker ps`_. 

Stop and remove that container  
You can type _`docker stop <container ID>` and remove it, or just use the _force_ flag  
`docker rm -f <container ID>`

Make these changes to the application:
`code src/static/js/app.js`
On line 56   
 `- <p className="text-center">No items yet! Add one above!</p>`    
`+ <p className="text-center">You have no items yet! Add one above!</p>`

Save, and update the image. (Don't forget the "." argument)
`docker build -t hello-docker . `  

The image is updated.  

Run a new container with the changes  
`docker run -dp 3000:3000 hello-docker`  

The app is running with the new changes on localhost
http://localhost:3000/

NOTE: If you look in Docker Desktop you will see that image was stubbed and not overwritten. Go ahead and removed this lame image `docker images` Get the image ID of <none> `docker rmi <image ID>` 
"rmi" removes images. If you run _`docker images`_ again or look in Docker Desktop, you will see it is gone. 

### Summary
This was the **image to container** workflow. You took an application. Copied it to an image you created (from a based image). Ran a container (process). More, you updated the application wrote that to (a new) image. And ran a new container process off of it.  

NOTE: Just guessing here, but it looks like Docker is using Singletons, which explains this kind of _one night stand_ behavior.

## Repetition is King
Writing is rewriting. Delete the running container
`docker ps` or `docker ps -a` // get the ID  
`docker rm -f <container ID>`  

Delete the image  
`docker images` // get the image ID  
`docker rmi <image ID>`  

**Seriously. Practice doing this a few times**  

COMMENT: The single reason why most documentation and tutorials _do not work_ is they did not run it through a UX team. The official Docker documentation on which this is based was  
- riddled with inaccuracies  
- non-linear and hard to follow  
- could not stay focused on what was important to the matter at hand  
- not clear for that matter, what "the matter at hand" was supposed to be  

Just like in programming, (or chess for that matter) so-called teachers confuse strategy with tactics, and fail to show the design on which a language is based.  

###

TO COME:  
### The all-important container to image workflow  
### Volumes and persistance  
### Bind Mounts  
### Shipping your images  

BIO-NOTE: Dylan Nirvana is a punk rocker, songwriter, UX enthusiast and programmer living somewhere on Earth. 
