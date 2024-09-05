# Deploying a Portfolio Website with a Custom Apache Server in Docker

This guide demonstrates how to build a portfolio website and deploy it using a custom Apache server in Docker.

Checkout the full [article](https://medium.com/@sharmarvellous/deploying-a-portfolio-website-with-a-custom-apache-server-in-docker-step-by-step-guide-6b11941bd8b3).

## Table of Contents
1. [Creating the Portfolio Website Files](#creating-the-portfolio-website-files)
2. [Creating a Dockerfile](#creating-a-dockerfile)
3. [Building the Docker Image](#building-the-docker-image)
4. [Running the Docker Container](#running-the-docker-container)
5. [Verifying the Running Container and Website](#verifying-the-running-container-and-website)
6. [Summary](#summary)

## Creating the Portfolio Website Files

To start, create a directory called `sample-web` that includes the following files:

- `index.html`: The main HTML file for the portfolio.
- `scripts.js`: A JavaScript file.
- `styles.css`: A CSS file for styling the webpage.

These files will be copied into the Apache web server’s default directory when building the Docker image.

## Creating a Dockerfile

Create a `Dockerfile` to define the steps for building your custom Docker image. The following steps detail how the `dockerfile3` should be structured:

### Step 1: Using Ubuntu as the Base Image

```
FROM ubuntu
```

This tells Docker to use the latest Ubuntu image from Docker Hub as the base for your custom image.

### Step 2: Installing Apache

```
RUN apt update && apt install apache2 -y
```

This command updates the package list and installs Apache2.

### Step 3: Copying the Website Files

Ensure your folder sample-web is in the same directory as the Dockerfile. Then, add the following command to copy the website files into the Apache web server's default directory:
```
COPY sample-web /var/www/html/
```

Explanation: This command copies the contents of the `sample-web` directory (which contains your HTML, JavaScript, and CSS files) into the Apache web server's default directory `(/var/www/html/)`. This ensures that your portfolio website is served by Apache.

### Step 4: Running Apache

```
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

Explanation: The `CMD` command runs Apache in the foreground, which is essential to keep the Docker container active. Without this command, the container would exit immediately after starting.
## Building the Docker Image

Once the `Dockerfile` is ready, build the Docker image using the following command:

```
docker build -t apachetest:v0 -f dockerfile3 .
```

### Explanation:

- `docker build`: Builds the Docker image from the Dockerfile.
- `-t apachetest:v0`: Tags the image as `apachetest:v0`.
- `-f dockerfile3`: Specifies to use the `dockerfile3`.
- `.`: Indicates that the Dockerfile and website files are in the current directory.

### Outcome:

The Docker image will contain Ubuntu, Apache, and your portfolio website.

## Running the Docker Container

To run the Docker container, use the following command:

```
docker run -d -p 9090:80 apachetest:v0
```

### Explanation:

- `docker run`: Starts a new Docker container.
- `-d`: Runs the container in detached mode (background).
- `-p 9090:80`: Maps port 9090 on the host to port 80 in the container.
- `apachetest:v0`: The Docker image to use.

### Outcome:

The container runs Apache, and you can access the website at `http://localhost:9090`.

## Verifying the Running Container and Website

To verify that the container is running, use:

```
docker ps
```

This command lists all running containers. The `apachetest:v0` container should be listed as active.

### Accessing the Website

Open a browser and go to `http://localhost:9090`. The portfolio website should be visible.

## Summary

This guide demonstrated how to:

1. Build a custom Docker image that serves a portfolio website with Apache.
2. Create a Dockerfile that installs Apache, copies website files, and runs the server.
3. Run the Docker container, map ports, and access the website locally.

This setup highlights Docker’s power in creating isolated, reproducible environments for hosting web applications.
