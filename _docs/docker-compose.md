## Docker Compose

Previously, we ran frontend and backend as an indivual containers. But this method is not scalable. In production deployments, we might have to run more than 2 containers. So managing indivual contianers becomes difficult. To address these issues, `docker-compose` comes to the resue.

## What is Docker Compose

Docker Compose is a tool that let's you run multiple containers using a declartive approach in `.yml` format.
In a single `.yml` file you can define multiple containers. This approach makes the development, deployment, and management of containerized application.

## Let's write Docker Compose File for Dev Environment
Usually as a best pratice, seperate docker-compose files are created for dev and prod. It's not a must do. But it keeps the dev and prod requirements seperate.

`docker-compose.dev.yml`

```
services:
  backend-flask:
    env_file:
      - ./backend-flask/.env
    build: ./backend-flask
    ports:
      - "5000:5000"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    env_file:
      - ./frontend-react-js/.env
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js
  post:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5436:5432'
    volumes: 
      - post:/var/lib/postgresql/data

  dynamodb-local:
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal        

volumes:
  post:
    driver: local      
``` 

Let's understand the parameters of `docker-compose.yml` file:
- services - under services you can define multiple containers. Every docker-compose file starts with services
- env _file - reference the .env file of your application
- build - reference the Dockerfile path
- ports - map host port with container port. It has {host_port}:{container_port} format
- volumes (under `post` service level) - there are several falvours of volumes in docker. I will describe it in a seperate section.
-Volumes (under root level) - defines which named volumes exist in your Docker environment. Without it, Docker Compose wouldn’t know pgdata is a named volume

## Docker Volumes

Dockers are ephimeral in nature. However, some applications needs to store the state of the application. In such cases, you need a persisent methods to store the data. This where Docker volumes shine.

Docker volumes are external storage managed by Docker. The volumes are stored outside the container’s writable layer.

There are 2 types of volumes you can define in your `docker-compose.yml`:

Bind Mounts: It maps (mirrors) the host path to the container. Use bind mounts for local developments where you want to see code changes instantly in the container without rebuilding the image every time. Format is `./host_path:/container_path`.

Named Volumes: These volumes are managed by Docker and stored in its own directory /var/lib/docker/volumes/.
These are specially designed for persistent data like databases etc. Format is `volume_name:/container_path`

Run the following command to run the docker compose file:
```
sudo docker compose -f docker-compose.dev.yml up -d
```

To rebuild the image along with running the docker compose file:
```
sudo docker compose -f docker-compose.dev.yml up -d --build
```

To stop the services:
```
sudo docker compose -f docker-compose.dev.yml down
```

There's an easy UI way to view logs and log in into containers from vscode. Install `Containers` extensions that is availabel in vscode.
1. Click `Containers` extension.
2. Right click on the container name.
3. Select **View Logs** to view logs.
4. Selectr **Attach Shell** to log in to the container.

To connect to the postgres database:
```
$ psql -h localhost -p 5436 -U postgres
```











