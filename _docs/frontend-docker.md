## Dockerize the Frontend Framework

It all starts with creating a `Dockerfile` (Yes, without extension) under the root of the `backend-flask' folder.

Add the following contents in `Dockerfile`:
```
FROM node:16.18

WORKDIR /frontend-react-js

COPY package.json .

RUN npm install

COPY . .

ENV PORT=3000

EXPOSE 3000

CMD ["npm", "start"]
```

Let's understand what each command does:

`FROM` - Every Dockerfile starts with `FROM` command. This is the command that refers to the base image which has all the necessary library, runtime, dependinces etc. Because we are using Flask framework, 'python:3.10-slim-buster' image is used here.

`WORKDIR` - Use this command to set the default directory inside container terminal. For example, when you log into ubuntu machine, and fireup the terminal, the prompt is at /home/{name}. Likewise when you login to container, the prompt is at /backend-flask. Also the subsiquent steps like copying files, running commands all happend from this directory.

`COPY` - Use this command to copy local files into container's `WORKDIR`
You might get a question like why are copying only requirements.txt? why not complete backend-flask folder? Reason being to load the react node modules all we need is `package.json`. So instead of copying the complete folder, which is unnecessary and time-consuming.


`RUN` - Use this command to run commands during the image build process. It is used for setup stuff to install packages, configure the system, or prepare the application environment. So basically when you run `docker build` command, the RUN command contents are loaded.

`ENV` - You can set any environment variables required for your application

`EXPOSE` - It is for documenation purpose only to let know users that app will be served on the mentioned port.

`CMD` - Use this command what to run when a container is started. So basically when you run `docker run` command, the CMD command contents are loaded.


To see container in action, you must follow two steps:
- first build
- then run

Build the docker:
```
$ sudo docker build -t frontend ./frontend-react-js
```
here -t means tag. tags have `image-name:{image_version}` format.
If you don't mention the tag, then it defaults to latest i.e `image-name:latest`

Run the docker:
```
$ sudo docker run -p 3000:3000 -d --name frontend frontend:latest
```

-p = publish port. It has {host_port}:{container_port} format.
-d = run the container in detatched node
--name = name of the container. If you don't specify it, Docker will assign a vague name.



