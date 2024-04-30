# Running a sample Next.js app with Docker

## What we do in normal development
- We have an OS (Windows, macOS, Linux)
- We have Node.js installed
- We have npm installed or (yarn, pnpm, etc.)
- We install the required dependencies using `npm install`
- We run the app using `npm run dev`
- Then the app is available at `http://localhost:3000` in the browser

## What we do with Docker
- We have Docker installed
- We have Dockerfile and docker-compose.yml
- We build the image of our application using `docker build -t sample-app .`
- We run the container using `docker run -p 3000:3000 sample-app`. `p` flag is for port mapping, `3000:8000` means map the port 3000(1st) of the host to the port 8000(2nd) of the container.
- Then the app is available at `http://localhost:3000` in the browser

## Dockerfile Contents
We will Discuss the contents of the Dockerfile
| Command | Description |
| --- | --- |
| `FROM` | means the base image for the application (node:20-alpine). Docker used Docker Hub by default, but it can be other repositories too. (example: `mcr.microsoft.com/dotnet/core/sdk:3.1 as build-env`) |
| `WORKDIR` | means the working directory for the application (`/app`). Even if the `/app` does not exits, then docker will create the directory |
| `COPY` | means copy the files from the current directory (the directory that contains the `Dockerfile`) to the working directory of the container(`/app`). <br> - Example: (`package.json package-lock.json .`) or (`./package*.json .`) [1st dot is the source (local computer)[`package.json` here] and 2nd dot is the destination(container's working directory - `/app`)] <br> - When copying multiple files from host to container always ends the container location with `/` means write (`/app/`) <br> - Also we can use `COPY ["package.json", "."]` |
| `ADD` | means copy the files from the current directory to the working directory of the container. (`./ .`) [1st dot is the source (local computer) and 2nd dot is the destination(container's working directory)] <br> - It is almost same as `COPY`, but it has 2 addtional features: <br> 1. We can add remote file in the container, <br> ex: `ADD ["https://github.com/jashezan/projects/package.json", "."]` <br> Adding a `zip` file to the directory will automatically uncompresses the zip into the directory. <br> Ex: `ADD file.zip .` | 
| `RUN` | means run the command in the container (`npm install`) or (`npm run build`) |
| `ENV` | means set the environment variable (`NODE_ENV=production`) |
| `EXPOSE` | means expose the port of the container (`3000`) |
| `USER` | means set the user (we are using limited previledged user here) for the container (node). <br> make a new user without root priviledge in alpine linux. 1st create a group `app`, then create it with <br> `adduser -S -G app app`, [`-S` means system user, `-G` means adding `app` user to `app` group, ] |
| `CMD` | means the command to run the application (`npm run start`). <br> This run only 1 time and it run as the default command <br> Shell Form: `CMD npm start` <br> Exec Form: `CMD ["npm", "start"]` <br> It can be over written though. |
| `ENTRYPOINT` | means the entry point of the application (`npm run start`) <br> It is almost similar to `CMD` but it can not be over written like `CMD` |

## Dockerfile
```Dockerfile
FROM node:20-alpine
RUN addgroup -g 1001 -S app && adduser -u 1001 -S app -G app
USER app
WORKDIR /app/
COPY --chown=app:app package.json .
COPY . .
RUN npm install
EXPOSE 3000
ENV custom=docker
CMD [ "yarn", "dev" ]
```

## Speed up the build
First learn the concept of layering in Docker. 
- Each command in the Dockerfile creates a layer. 
- So, basically the Dockerfile (App Image) is a set of layers.
- If the layer is not changed, then it will use the cache. 
- If the layer is changed, then it will rebuild the layer. 
- No need to use `--no-cache` to build the image without cache. `docker build --no-cache -t sample-app .`
- So, we need to put the commands that are less likely to change at the top of the Dockerfile. 
- So, the commands that are more likely to change should be at the bottom of the Dockerfile. 
- So, the `COPY . .` should be at the bottom of the Dockerfile.
- `docker history sample-app` will show the history of the layers of the image.

### Tips
- Use `.dockerignore` file to ignore the files that are not needed in the container.
- Use `--chown=app:app` to change the ownership of the files to the user `app` in the container.
- From top of the Dockerfile to the bottom, the commands are executed in the order of the Dockerfile.
- So, the commands that are less likely to change should be at the top of the Dockerfile.
- The commands that are more likely to change should be at the bottom of the Dockerfile.
- Dockerfile(top -> bottom): Stable instructions -> Changing instructions

## Optimizing the Dockerfile
- Use multi-stage builds to reduce the size of the image.
- Use `.dockerignore` file to ignore the files that are not needed in the container.

```Dockerfile
# Stage 1: Build the app
FROM node:20-alpine as build
WORKDIR /app/
COPY package.json .
COPY . .
RUN npm install
RUN npm run build

# Stage 2: Run the app
FROM node:20-alpine
RUN addgroup -g 1001 -S app && adduser -u 1001 -S app -G app
USER app
WORKDIR /app/
COPY --from=build /app/package.json .
COPY --from=build /app/.next .next
COPY --from=build /app/public public
RUN npm install --only=production
EXPOSE 3000
ENV custom=docker
CMD [ "yarn", "start" ]
```

## Summary
- We have learned how to run a sample Next.js app with Docker.
- We have learned the contents of the Dockerfile.
- We have learned how to speed up the build.
- We have learned how to optimize the Dockerfile.
- We have learned how to use multi-stage builds.
- We have learned how to use `.dockerignore` file.
- We have learned how to use `--chown=app:app` to change the ownership of the files to the user `app` in the container.
- We have learned the order of the commands in the Dockerfile.
- We have learned the concept of layering in Docker.
- We have learned how to reduce the size of the image using multi-stage builds.
