# Defaut run commande
  
    docker run -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock jwilder/nginx-proxy
    
Container avec virtual host
    
    docker run -d -e VIRTUAL_HOST=sub.domaine.com nom_mon_image

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
