FROM node:alpine 
# Base image - node:alpine
# Generally we have to install linux then node and then our app
# But,generally node image already comes with linux and node installed
# we can use node:ubuntu which is based on ubuntu
# or node:alpine which is based on alpine
# or node:debian which is based on debian
# we are using node:alpine because alpine is a very small and lightweight linux distro
# so, it will make our image size small
# and it is also secure and fast
# so, we have done 2 step of our process to run our app

COPY . /app
# Copy all the files from current directory to /app directory in the image
# . means current directory (out local directory)
# /app is the directory in the image (inside the container)
# so, we are copying all the files from our local directory to the /app directory in the image
# so, we have done 3rd step of our process to run our app

WORKDIR /app
# Set the working directory to /app
# this working directory is inside the container
# its like opening the terminal in /home/user directory in out local computer
# when we open the terminal in /home/user directory then we can directly run the commands in that directory
# so, we are setting the working directory to /app directory in the image
# all the commands will assume that they are running in /app directory
# so all the commands below will be executed in relative to /app directory

CMD ["node", "index.js"]
# Run the command node index.js
# this command will run when the container starts
# CMD means command to run when the container starts inside the container
# the array after CMD is the command to run and its arguments
# the 1st element is to run node command and 2nd element is to run index.js file with node command
# so, we have done 4th step of our process to run our app 