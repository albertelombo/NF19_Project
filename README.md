# NF19 Project : Giftcard

This is a guide to launch the wordpress test environment for our giftcard CMS. Said environment is made of two services which are wordpress and MySQL to serve the wordpress. We put at your disposal two images for each service already configured for our website :

+ [albertelombo/giftcard_db](https://hub.docker.com/repository/docker/albertelombo/giftcard_db "MySQL database")
+ [albertelombo/giftcard_wp](https://hub.docker.com/repository/docker/albertelombo/giftcard_wp "Wordpress website")

## Prerequisites

1. Make sure to install :
    + [Docker Engine for your platform](https://docs.docker.com/engine/install/ "Docker Engine doc")
    + [Docker compose ](https://docs.docker.com/compose/install/)

2. Make sure no service is running on port 80

## Launching the test environment

1. First you must create in a folder dedicated to the test a `docker-compose.yml` file such as the following

    ```yaml
    services:
        db:
            # We use a mariadb image which supports both amd64 & arm64 architecture
            #image: mariadb:10.6.4-focal
            # If you really want to use MySQL, uncomment the following line
            #image: mysql:8.0.27
            #command: '--default-authentication-plugin=mysql_native_password'
            image: albertelombo/giftcard_db
            volumes:
            - db_data:/var/lib/mysql
            restart: always
            environment:
            - MYSQL_ROOT_PASSWORD=somewordpress
            - MYSQL_DATABASE=wordpress
            - MYSQL_USER=wordpress
            - MYSQL_PASSWORD=wordpress
            expose:
            - 3306
            - 33060
        wordpress:
            image: albertelombo/giftcard_wp
            #image: wordpress:latest
            volumes:
            - wp_data:/var/www/html
            ports:
            - 80:80
            restart: always
            environment:
            - WORDPRESS_DB_HOST=db
            - WORDPRESS_DB_USER=wordpress
            - WORDPRESS_DB_PASSWORD=wordpress
            - WORDPRESS_DB_NAME=wordpress
            - COMPOSE_PROJECT_NAME=giftcard
    volumes:
        db_data:
        wp_data:
    ```
2. Launch the different services
    + Windows
        ```bash
        docker compose up -d
        ```
    + Linux or MacOS
        ```bash
        sudo docker compose up -d
        ```
3. Login if necessary with following credentials
    + username: albertelombo
    + password: NF19Pass
