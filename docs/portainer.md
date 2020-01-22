Persiapan
Pastikan network traefik-public sudah ada
a. buat domain untuk dashboard portainer
contoh:  
portainer-arsip.karangasemkab.go.id

```
export DOMAIN=portainer-arsip.nama_simpul.go.id \
&& export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}') \
&& docker node update --label-add portainer.portainer-data=true $NODE_ID
```
b. arahkan domain tersebut ke host docker (Private/Puclic IP)

download/buat docker-compose untuk portainer simpan dalam file portainer.yml
```
mkdir -p /opt/portainer \
&& cd /opt/portainer/ \
&& curl https://raw.githubusercontent.com/gitsikn/atom/stable/2.5.x/docker/portainer.yml -o portainer.yml
```
untuk mode docker-swarm  
nano portainer.yml
```
version: '3.3'

services:
  agent:
    image: portainer/agent
    environment:
      AGENT_CLUSTER_ADDR: tasks.agent
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/lib/docker/volumes:/var/lib/docker/volumes
    networks:
      - agent-network
    deploy:
      mode: global
      placement:
        constraints:
          - node.platform.os == linux

  portainer:
    image: portainer/portainer
    command: -H tcp://tasks.agent:9001 --tlsskipverify
    volumes:
      - portainer-data:/data
    networks:
      - agent-network
      - traefik-public
    deploy:
      placement:
        constraints:
          - node.role == manager
          - node.labels.portainer.portainer-data == true
      labels:
        - traefik.frontend.rule=Host:${DOMAIN?Variable DOMAIN not set}
        - traefik.enable=true
        - traefik.port=9000
        - traefik.tags=traefik-public
        - traefik.docker.network=traefik-public
        # Traefik service that listens to HTTP
        - traefik.redirectorservice.frontend.entryPoints=http
        - traefik.redirectorservice.frontend.redirect.entryPoint=https
        # Traefik service that listens to HTTPS
        - traefik.webservice.frontend.entryPoints=https

networks:
  agent-network:
    attachable: true
  traefik-public:
    external: true

volumes:
  portainer-data:
```
untuk single node
```
version: '2'

services:
  portainer:
    image: portainer/portainer
    command: -H unix:///var/run/docker.sock
    restart: always
    ports:
      - 9000:9000
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data

volumes:
  portainer_data:
```

### deploy portainer swarm mode
```
docker stack deploy -c portainer.yml portainer
```

### deploy portainer single node
```
docker-compose -f portainer.yml up -d
```

### Cek
```
docker ps
docker logs portainer_container_id
```

### Cek User Interface
https://portainer.domain_name
