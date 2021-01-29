# Dockerized Nginx with SSL

Project for automatically creating **SSL certicates** for dockerized apps using **Nginx + Docker + LetsEncrypt**.

## Prerequisites

Make sure you have installed these:
- [Docker and Docker Compose](https://phoenixnap.com/kb/install-docker-compose-on-ubuntu-20-04) - Will install all the required packages and software.

## Installation

Make a copy of `.env.example` file named `.env`

```shell script
cp .env.example .env
```

---

Environment variables:
- `DEFAULT_EMAIL` - default email, where we get notifications from **LetsEncrypt**
- `NGINX_PROXY_NETWORK` - Docker network, which we will use find Dockerized apps to add SSL certificates. All our Dockerized apps, that we want to add SSL certificates, should be inside this network.

```dotenv
DEFAULT_EMAIL=user@example.com
NGINX_PROXY_NETWORK=nginx-proxy
```

---

Create network with the name, that we have in `NGINX_PROXY_NETWORK` environment variable.

```shell script
docker network create nginx-proxy
```

---

Build the Docker image

```shell script
docker-compose build
```

## Running

Start
```
docker-compose up
```

Stop
```
docker-compose down
```

## Usage

- Dockerize your app and create `docker-compose.yml` file.
- Open `docker-compose.yml`
- Find the service, that we want to add **SSL certificates**
- Add these values into `environment:` section:

    ```docker-c
    service_name:
    ...
    environment:
        ... Other environment variables ...
        VIRTUAL_HOST: <domain_of_our_service>
        VIRTUAL_PORT: <port_of_app_inside_docker_network>
        LETSENCRYPT_HOST: <same_as_VIRTUAL_HOST>
        LETSENCRYPT_EMAIL: <email_address_for_letsencrypt_notifications>
    ```

- Add networks section inside service

    ```
    service_name:
    ...
    environment:
        ...
    networks:
        - proxy
    ```

- Add the network at the end of `docker-compose.yml`

    ```
    version "3"

    services:
    ...

    networks:
    proxy:
        external:
        name: <NGINX_PROXY_NETWORK_value_from_.env>
    ```

- Restart **Dockerized Nginx**


## Authors
- Adi Sabyrbayev [Github](https://github.com/madrigals1), [LinkedIn](https://www.linkedin.com/in/madrigals1/)