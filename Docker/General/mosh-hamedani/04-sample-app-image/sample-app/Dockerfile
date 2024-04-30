FROM node:20.12-alpine3.18
# using the node image as the base image

RUN addgroup -g 1001 -S app && adduser -u 1001 -S app -G app
# creating a user "app" with the group "app" and the user id 1001

USER app
# switching to the user "app"

WORKDIR /app/
# setting the working directory

COPY --chown=app:app package*.json .
# copying the package.json file to the working directory and changing the ownership to the user "app"

RUN npm install
# installing the dependencies

COPY . .
# copying the rest of the files to the working directory

EXPOSE 3000
# exposing the port 3000

ENV custom=docker
# setting an environment variable

CMD [ "yarn", "dev" ]
# running the command "yarn dev" when the container starts