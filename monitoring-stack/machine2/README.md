# Machine 2 Setup

## Installation du site web
```
sudo apt update && sudo apt install -y nginx
sudo rm -rf /var/www/html/*
sudo cp -r ./site/* /var/www/html/
sudo cp ./nginx.conf /etc/nginx/nginx.conf
sudo systemctl restart nginx
sudo systemctl enable nginx
```

## Installation de Node Exporter
```
useradd -rs /bin/false nodeexp
cd /tmp
curl -LO https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-1.8.2.linux-amd64.tar.gz
tar xvf node_exporter-*.tar.gz
sudo cp node_exporter-*/node_exporter /usr/local/bin/
sudo cp ./node_exporter.service /etc/systemd/system/node_exporter.service
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
```

## Vérification

- Site web : http://<IP_MACHINE2>  
- Metrics Node Exporter : http://<IP_MACHINE2>:9100/metrics  


## Sécurité & recommandations

- Pare-feu :
  - Autoriser uniquement : `80`, `9100`, `9090`, `9093`, `3000`.
- Réseau docker isolé `monitoring`.
- Variables sensibles (mots de passe Grafana) dans `.env` si besoin.
- HTTPS possible via Traefik ou Nginx reverse proxy sur Machine 1.

