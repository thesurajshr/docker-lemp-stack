# Docker Compose Configuration for Nginx, MariaDB, PHP, and optional PhpMyAdmin

This `docker-compose.yml` file sets up a development environment with Nginx, MariaDB, and PHP. It also provides an optional PhpMyAdmin container for managing the MariaDB database.

## Prerequisites

Before using this configuration, you need to have the following installed on your system:

-   Docker
-   Docker Compose

## Usage

1. Clone this repository to your local machine:

    ```bash
    git clone https://github.com/thesurajshr/docker-lemp-stack
    cd docker-lemp-stack
    ```

    This will create a folder named `your-repo` (or the name of your repository) in the current directory and download the repository's contents, including the `docker-compose.yml` file.

2. **Customize the environment variables** in the `.env` file to match your project requirements.
3. **Build and Start the Environment**: After customizing the `docker-compose.yml`, `.env` and ensuring the required files and directories are in place, you can build and start the environment using Docker Compose

## Configuration

The `docker-compose.yml` file contains the following services:

### Nginx Service

-   Image: `nginx:latest`
-   Container Name: `${APP_NAME}_nginx`
-   Ports: Exposes port `80` on the host system to port `80` in the Nginx container.
-   Volumes:
    -   `./conf/nginx/nginx.conf:/etc/nginx/nginx.conf`: Mounts Nginx configuration file.
    -   `./conf/nginx/includes:/etc/nginx/includes`: Mounts Nginx includes directory.
    -   `./conf/nginx/sites-available:/etc/nginx/sites-available`: Mounts Nginx sites-available directory.
    -   `./logs/nginx:/var/log/nginx`: Mounts Nginx log directory.
    -   `./conf/nginx/html:/usr/share/nginx/html`: Mounts the Nginx HTML root directory.
    -   `./www/html:/var/www/html`: Mounts your application code or website into Nginx.
-   Logging: JSON file logging with a maximum size of 10 MB and 5 log files.
-   Dependencies: Depends on the `php` service to ensure it starts after PHP.
-   Restart Policy: The Nginx container will restart unless explicitly stopped.
-   Network: Connected to the `app_net` network.

### MariaDB Service

-   Image: `mariadb:latest`
-   Container Name: `${APP_NAME}_mariadb`
-   Ports: Exposes port `3306` on the host system to port `3306` in the MariaDB container.
-   Volumes:
    -   `./logs/mysql:/var/log/mysql`: Mounts MariaDB log directory.
-   Environment Variables: Configures the MariaDB root password and database information.
-   Logging: JSON file logging with a maximum size of 10 MB and 5 log files.
-   Network: Connected to the `app_net` network.

### PHP Service

-   Build: Builds the PHP container using the Dockerfile at `./conf/php/docker/Dockerfile`.
-   Container Name: `${APP_NAME}_php`
-   Volumes:
    -   `./logs/php:/var/log/php`: Mounts PHP log directory.
    -   `./www/html:/var/www/html`: Mounts your application code or website into PHP.
    -   `./conf/php/ini/${ENVIRONMENT}/php.ini:/usr/local/etc/php/php.ini`: Mounts PHP configuration file based on the specified environment.
-   Environment Variables: Sets database connection information for PHP.
-   Ports: Exposes port `9000` on the host system to port `9000` in the PHP container.
-   Logging: JSON file logging with a maximum size of 10 MB and 5 log files.
-   Network: Connected to the `app_net` network.

### PhpMyAdmin Service (Optional)

-   Image: `phpmyadmin/phpmyadmin`
-   Container Name: `${APP_NAME}_phpmyadmin`
-   Platform: Linux/amd64
-   Ports: Exposes port `8080` on the host system to port `80` in the PhpMyAdmin container.
-   Environment Variables: Configures PhpMyAdmin to connect to the MariaDB instance.
-   Network: Connected to the `app_net` network.

### Networks

-   app_net: This is the custom bridge network connecting all the services. It allows containers to communicate with each other using their container names as hostnames.

## Customization

-   Replace `${APP_NAME}` with your desired name for the containers (e.g., myapp).
-   Configure the environment variables as per your project's requirements.
-   Place your Nginx configuration files, PHP code, and other necessary files in the specified directories.

## Start the Environment

To start the environment, navigate to the directory containing the `docker-compose.yml` file and run the following command:

    docker-compose up -d

This will build the PHP container (if not already built) and start all the services in the background.

To stop the environment, run:

    docker-compose down
