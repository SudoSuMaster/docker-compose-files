MyProject - Dockerized PHP & MySQL Application

Overview

This guide will walk you through setting up a simple Dockerized PHP & MySQL application using docker-compose and nginx.

Project Structure

myproject/
│── docker-compose.yml
│── nginx/
│   └── default.conf
│── php/
│   └── Dockerfile
│── www/
│   └── index.php

Prerequisites

Ensure you have the following installed on your system:

Docker

Docker Compose

Configuration Details

1. docker-compose.yml

This file defines the services for our application.

version: '3.8'

services:
  nginx:
    image: nginx:latest
    container_name: nginx_server
    restart: always
    ports:
      - "8080:80"
    volumes:
      - ./www:/var/www/html
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php
      - mysql
    networks:
      - app_network

  php:
    build: ./php
    container_name: php_fpm
    restart: always
    volumes:
      - ./www:/var/www/html
    networks:
      - app_network

  mysql:
    image: mysql:latest
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - app_network

volumes:
  mysql_data:

networks:
  app_network:

Explanation:

nginx: Acts as the web server, serving PHP files via FastCGI.

php: A container running PHP-FPM to execute PHP scripts.

mysql: A MySQL database container.

Volumes: Persist MySQL data even if the container is restarted.

Networks: Ensures communication between services.

2. php/Dockerfile

This file defines how the PHP container is built.

FROM php:8.2-fpm

# Install required PHP extensions  
RUN docker-php-ext-install pdo pdo_mysql mysqli

Explanation:

Uses php:8.2-fpm as the base image.

Installs necessary PHP extensions (pdo, pdo_mysql, mysqli).

3. nginx/default.conf

This file configures the Nginx web server.

server {
    listen 80;
    server_name localhost;
    root /var/www/html;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }
}

Explanation:

Serves files from /var/www/html.

Passes .php files to the PHP-FPM container (php:9000).

Denies access to hidden files (e.g., .htaccess).

4. www/index.php

A basic PHP script to test MySQL connectivity.

<?php
$servername = "mysql";
$username = "user";
$password = "password";
$dbname = "mydatabase";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
echo "Connected successfully to MySQL!";
?>

Explanation:

Connects to the MySQL container using environment variables from docker-compose.yml.

Displays a success message if the connection is successful.

Running the Application

Navigate to the project directory:

cd myproject

Build and start the containers:

docker-compose up -d

Access the application in your browser at:

http://localhost:8080

To stop the containers:

docker-compose down

Conclusion

This setup provides a simple yet functional PHP development environment using Docker. You can extend it by adding additional services such as Redis, PostgreSQL, or even integrating a frontend framework.
