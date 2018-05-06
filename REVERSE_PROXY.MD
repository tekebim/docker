# Defaut run commande
  
    docker run -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock jwilder/nginx-proxy
    
Container avec virtual host
    
    docker run -d -e VIRTUAL_HOST=sub.domaine.com nom_mon_image
