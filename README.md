# Recipe Blog - Using Node.js and MongoDB

## Create .env file
Create a .env file to store your MongoDB database credentials

Example only (copy the full connection string from MongoDB):
```
MONGODB_URI = mongodb+srv://<username>:<password>@cluster0.6m5cz.mongodb.net/
```

## Installation
To run this project, install it locally using npm:

```<img width="1891" height="969" alt="Screenshot 2026-01-15 173731" src="https://github.com/user-attachments/assets/a2d0bc01-7f3f-4472-a71c-e17b86d3dddd" />

$ npm install
$ npm start
```
# 🐳 Docker Setup Guide

This project is containerized using Docker, allowing you to run the Node.js application and MongoDB database without installing them locally on your machine.

## Prerequisites
* **Docker Desktop**: Ensure Docker is installed and running. [Download here](https://www.docker.com/products/docker-desktop/).

## 🚀 Quick Start

1.  **Open your terminal** in the project root folder.
2.  **Build and Start** the containers:
    ```bash
    docker compose up --build
    ```
    *(Note: If you are using an older version of Docker, use `docker-compose up --build`)*

3.  **Wait for the logs**: You will see the message `Connected to MongoDB` and `Listening to port 3000`.

## 🌐 Accessing the Application

* **Website:** [http://localhost:3000](http://localhost:3000)

## 🗄️ Managing the Database

Even though MongoDB is running inside Docker, port `27017` is exposed so you can manage it with tools like **MongoDB Compass**.

* **Connection String:** `mongodb://localhost:27017`
* **Database Name:** `RecipeBlog`

> **Important:** The Docker database uses a separate volume from your local install. **It will start empty.** You may need to uncomment the "seeding" functions in `server/controllers/recipeController.js` one time to populate it with starter recipes.

## 🛑 Stopping the App

* **Stop temporarily:** Press `Ctrl + C` in the terminal.
* **Stop and remove containers:**
    ```bash
    docker compose down
    ```

## ⚠️ Troubleshooting

**"Port already in use" Error:**
If you cannot start Docker, it is likely because your local version of Node.js or MongoDB is still running.
1.  Stop your local terminal (`Ctrl + C`).
2.  Stop the local MongoDB Service (if running).
3.  Run `docker compose up` again.

