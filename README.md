# App Containerization Project

This project will demonstrate on how to Dockerize the ephermeral micro-blogging application.
I will be documenting the steps I followed and concepts of Docker.


### What is Containerization Technology? Why someone must use it?

Containers came into existence because of the classic well known issue "it works in my system". As a developer when you're bulding applications you might have set of frameworks, libraries, dependenices, and runtime enivronments configured into your system. Similarly the stage and prod environments might have standard environments configured. When you deploy your code into stage or prod and there's a mismatch with the configuration, then the application might thrown an error. Contianers addressed these problems by providing portability.


### How different continers are from VMs

Unlike VMs, containers share the same operating system kernel and isolate the application processes from the rest of the system so the whole thing can be migrated, opened, and used across development, testing, and production configurations. 

### What is Docker? How's it realted to containers?

Docker is a tool that packages the software as containers. Inside containers, the app will have it's frameworks, libraries, dependenices, and runtime enivronments.

### Understand the Ephermeral Micro-Blogging Application Stack

The application which we are going to containrize is a micro-blogging platform application (ephemeral in nature) that allows users to post updates that automatically expire after a period of time. The application has the following stack:

- Frontend - React
- Backend - Flask
- Feed Conversations - Postgres
- Private Messages - DynomoDB

### Running the application locally

After having the application stack understanding, it's time to run the app locally (without database). By running the app locally we will understand the application building steps which we help us to simulate the same steps when contanierizing the application.

First things first, you must create a `.env` file under the root of backend-flask and frontend-react-js folders. What needs to go inside `.env` file ? refer to `.env.example` file and add the same variables.

To run the backend application follow these steps:
```
cd backend-flask
python3 -m flask run
```
Go to your browser and enter localhost:5000/api/activities/home

> port **5000** is the default port on which Flask runs

> 5000/api/activities/home - is the API end-point that shows the data

<img src="images/5000.png" alt="Alt text" width="400"/>


To run the backend application follow these steps:
```
cd frontend-react-js
npm install #installs all required packages from package
npm start
```

Tada! localhost:3000 automatically opens in the browser.

<img src="images/3000.png" alt="Alt text" width="500"/>
