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
- sudo certbot -d meta.les-cles.com --manual --preferred-challenges dns certonly
- nginx config
- nano /etc/nginx/sites-available/sorbonne.com
```
server {
    listen 443 ssl;
    server_name meta.les-cles.com;

    ssl_certificate /etc/letsencrypt/live/meta.les-cles.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/meta.les-cles.com/privkey.pem;
    ssl_session_cache builtin:1000 shared:SSL:10m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4;
    ssl_prefer_server_ciphers on;

    location / {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_pass https://localhost:3001;
    }
}
```
- nginx -t
- systemctl restart nginx


- tout les 3 mois (certbot renew)

