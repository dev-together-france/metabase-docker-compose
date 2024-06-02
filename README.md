# metabase-docker-compose

## Installation du serveur de prod

- Installer Git
- Installer Docker (https://docs.docker.com/engine/install/debian/ )
- Créer une clée SSH, sur le serveur, pour cloner Git (ssh-keygen)
- Ajouter la clé public à Github du drive (cat id_github.pub)
- Ajouter la clé SSH au system (ssh-add id_github)
- Cloner le projet (mkdir sources && git clone git@github.com:dev-together-france/...)

## Mise en place du certificat

- apt install certbot python3-certbot-apache -y
- certbot certonly --manual
- nginx config
- nano /etc/nginx/sites-available/sorbonne.com
```
server {
    listen 300 ssl;
    server_name effectifsdlm.hosted.lip6.fr;

    ssl_certificate /root/sources/sorbonne-rh/api/src/ssl/fullchain.pem;
    ssl_certificate_key /root/sources/sorbonne-rh/api/src/ssl/privkey.pem;

    location / {
       proxy_pass http://132.227.66.5:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
- nginx -t
- systemctl restart nginx


- tout les 3 mois (certbot renew)

