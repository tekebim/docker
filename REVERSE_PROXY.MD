> Penser à vider le cache navigateur pour les tests ( ou via autre navigateur )


# Defaut run commande
  
    docker run -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock jwilder/nginx-proxy
    
Container avec virtual host
    
    docker run -d -e VIRTUAL_HOST=sub.domaine.com nom_mon_image


# Create un network

    docker network create nginx_default

# Docker-compose

Test 1 :

- Un seul fichier
```yml
version: '2'

services:

  nginx-proxy:
    restart: always
    image: jwilder/nginx-proxy
    container_name: nginx-proxy
    ports:
      - "80:80"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro

  static:
    restart: always
    image: mystatic:v1
    container_name: static-v1
    ports:
    - "1234:80"
    environment:
      VIRTUAL_HOST: sub.domain.com
      VIRTUAL_PORT: 1234
      
networks:
  default:
    external:
      name: nginx_default
```

Deux fichiers

 - docker-compose.yml > nginx-proxy
 
 ```yml
version: '2'

services:

  nginx-proxy:
    restart: always
    image: jwilder/nginx-proxy
    container_name: nginx-proxy
    ports:
      - "80:80"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro
      
networks:
  default:
    external:
      name: nginx_default
```

- Autre docker-compose.yml

```yml
version: '2'

services:

  db:
    image: mysql
    environment:
      - MYSQL_ROOT_PASSWORD=mypass
    volumes:
      - db-data:/var/log/mysql
  wp:
    image: wordpress
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_PASSWORD=mypass
    volumes:
      - wp-data:/var/www/html
    ports:
      - "1235:80"
    environment:
      VIRTUAL_HOST: sub.domain.com
      VIRTUAL_PORT: 1235
volumes:
   db-data:
      driver: local
   wp-data:
      driver: local

networks:
  default:
    external:
      name: nginx_default
```
