 MEMO ubuntu 14.04 upgrade to 16.04


## pre setup
buntu

sudo apt-add-repository ppa:zanchey/asciinema
sudo apt-get update
sudo apt-get install asciinema


## compose

```file: docker-compose.yml
version: "2"
services:
  wp_web_data:
    image: python:2.7-alpine
    volumes:
      - /opt/datastore:/var/www/html
  wordpress:
    image: wordpress:latest
    ports:
      - "80:80"
    depends_on:
      - db
      - wp_web_data
    volumes_from:
      - wp_web_data
    environment:
      WORDPRESS_DB_HOST: "db:3306"
    networks:
      - flat-network
    env_file: .env
  db:
    image: mysql:5.7
    volumes:
      - "db-data:/var/lib/mysql"
    networks:
      - flat-network
    env_file: .env
volumes:
  db-data:
networks:
  flat-network:
```

