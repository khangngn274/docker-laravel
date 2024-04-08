# docker-laravel

Course for create docker compose for laravel project


#add server container with docker files
  server:
    build:
      context: .
      dockerfile: dockerfiles/nginx.dockerfile
    ports: 
      - '8000:80'
    volumes: 
      - ./src:/var/www/html
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on: 
      - php
      - mysql
#add php container with docker files
  php:
    build: 
      context: .
      dockerfile: dockerfiles/php.dockerfile
    volumes:
      - ./src:/var/www/html:delegated
#add mysql container with docker files
  mysql: 
    image: mysql:8.0
    env_file:
      - ./env/mysql.env
  composer:
    build: 
      context: .
      dockerfile: dockerfiles/composer.dockerfile
    volumes:
      - ./src:/var/www/html
#add artisan container with docker files
  artisan:
    build:
      context: .
      dockerfile: dockerfiles/php.dockerfile
    volumes:
      - ./src:/var/www/html
    entrypoint: ['php', '/var/www/html/artisan']
#add npm container with docker files
  npm:
    image: node:14
    working_dir: /var/www/html
    entrypoint: ['npm']
    volumes:
      - ./src:/var/www/html