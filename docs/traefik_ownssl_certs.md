Created directories:
```
sudo mkdir -p /opt/traefik/certs
```
copy file *.crt dan *.key ke directory /opt/traefik/certs

```
sudo chmod 755 /opt/traefik
sudo chmod 750 /opt/traefik/certs
```

```
chmod 644 /opt/traefik/certs/publik.crt
chmod 600 /opt/traefik/certs/private.key 
```
membuat file *.toml


persistant env variable in centos 

add env variable on file /etc/environment

EMAIL=jatnikonm@anri.go.id

source /etc/environment
