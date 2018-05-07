# PHP + MYSQL + PHPMYADMIN ( HTTPS )

```yml
version: '2'
services:
  phpmyadmin-a:
    restart: always
    image: phpmyadmin/phpmyadmin
    links:
      - db-a:dba
    environment:
      VIRTUAL_HOST: db.sub.domain.com
      PMA_HOST: dba
    networks:
      - default
      - nginx_default

  db-a:
    restart: always
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: wordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    volumes:
      - ./shared_db_volume:/var/lib/mysql/

  web-a:
    image: richarvey/nginx-php-fpm:php7
    environment:
      - VIRTUAL_HOST=db.domain.com
      - LETSENCRYPT_HOST=db.domain.com
      - LETSENCRYPT_EMAIL=contact@domain.com
    expose:
      - '80'
    volumes:
      - ./html:/var/www/html/
    restart: always
    networks:
      - nginx_default

networks:
  nginx_default:
    external:
      name: nginx_default
```
