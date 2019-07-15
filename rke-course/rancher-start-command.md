# Rancher Start Command

## Create Rancher Folder

sudo mkdir /rancher

## Start rancher with mounted folder

### This mount will allow you to use the same database folder each time you run rancher without losing your data.

    docker run -d --restart=unless-stopped \
    -p 80:80 -p 443:443 \
    -v /host/certs:/container/certs
       (local machine) : (container)




## Rancher start command using self signed certs

    docker run -d --restart=unless-stopped \
    -p 80:80 -p 443:443 \
    -v /host/certs:/container/certs \
    -e SSL_CERT_DIR="/container/certs" \
    rancher/rancher:latest