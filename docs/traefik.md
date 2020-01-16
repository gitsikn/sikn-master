### swarm mode

### Persiapan  
referensi:

https://docs.traefik.io/user-guide/docker-and-lets-encrypt/   
https://github.com/wekan/wekan/wiki/Traefik-and-self-signed-SSL-certs


membuat network yang digunakan oleh traefik dan container
```
docker network create --driver=overlay traefik-public
docker network ls
```
membuat env variable, sesuaikan parameter
```
export EMAIL=nama_email &&
export DOMAIN=nama_root_domain &&
export USERNAME=admin &&
export PASSWORD=changethis &&
export HASHED_PASSWORD=$(openssl passwd -apr1 $PASSWORD) &&
echo $HASHED_PASSWORD &&
export CONSUL_REPLICAS=0 &&
export TRAEFIK_REPLICAS=1
```

Download file konfigurasi traefik
```
mkdir /opt/traefik \
&& cd /opt/traefik \
&& curl -L https://raw.githubusercontent.com/gitsikn/atom/stable/2.5.x/docker/traefik.yml -o traefik.yml
```
deploy traefik-consul
```
docker stack deploy -c traefik.yml traefik-consul
docker stack ps traefik-consul
```
melihat log
```
docker service logs traefik-consul_traefik
```
### Buat lokal DNS

C:\Windows\System32\drivers\etc  

test web ui traefik
https://traefik-arsip.nama_domain

test web-ui consul
https://consul-arsip.nama_domain


### single node (optional)
```
docker network create web
mkdir -p /opt/traefik

touch /opt/traefik/docker-compose.yml
```
```
version: '2'

services:
  traefik:
    image: traefik
    restart: always
    ports:
      - 80:80
      - 443:443
    networks:
      - web
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /opt/traefik/traefik.toml:/traefik.toml
      - /opt/traefik/acme.json:/acme.json
    container_name: traefik

networks:
  web:
    external: true
```


---
```
touch /opt/traefik/acme.json && chmod 600 /opt/traefik/acme.json

touch /opt/traefik/traefik.toml
```
```
debug = false

logLevel = "ERROR"
defaultEntryPoints = ["https","http"]

[entryPoints]
  [entryPoints.http]
  address = ":80"
    [entryPoints.http.redirect]
    entryPoint = "https"
  [entryPoints.https]
  address = ":443"
  [entryPoints.https.tls]

[retry]

[docker]
endpoint = "unix:///var/run/docker.sock"
domain = "dispustaka.karangasemkab.go.id"
watch = true
exposedByDefault = false
```
```
[acme]
email = "eka.wd88@gmail.com"
storage = "acme.json"
entryPoint = "https"
onHostRule = true
[acme.httpChallenge]
entryPoint = "http"
```

docker-compose -f docker-compose.yml up -d
```
