## Melihat Spesifikasi server

melihat cpu
```
lscpu
```

informasi memory

```
free -m
```
informasi disk
```
df -h
```
informasi OS
```
lsb_release -a
```
### set domain name
```
export USE_HOSTNAME=nama_domain.go.id \
&& echo $USE_HOSTNAME > /etc/hostname \
&& hostname -F /etc/hostname
```
### Install the latest updates

### Ubuntu
```
apt-get update && apt-get upgrade -y
```
CentOS
```
yum update -y \
yum upgrade
```
### 1. download dan install docker  
```
mkdir -p /opt/docker \
&& cd /opt/docker \
&& curl -fsSL get.docker.com -o get-docker.sh \
&& CHANNEL=stable sh get-docker.sh
```
### Configure Docker to start on boot
```
sudo systemctl enable docker && sudo systemctl start docker
```

### 2. install docker-compose (Skip jika mengggunakan docker swarm)

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

### test docker installation
```
docker --version \
&& docker run hello-world
```

### 3. test instalasi docker-compose
```
docker-compose --version
```
### Set Docker Experimental

```
echo $'{\n    "experimental": true\n}' | sudo tee /etc/docker/daemon.json;
systemctl restart docker
```
cek status experimental
```
docker version -f '{{.Server.Experimental}}'
```
Tuning virtual machines
Edit **/etc/sysctl.conf** dengan menambahkan  
```
vm.max_map_count=262144
```
atau eksekusi perintah

```
sudo sysctl -w vm.max_map_count=262144
```

Cek vm.max_map_count
```
sysctl vm.max_map_count
```
```
mkdir -p /etc/systemd/system/docker.service.d/ \
&& touch /etc/systemd/system/docker.service.d/10-machine.conf \
&& sudo sed -i '/ExecStart=\/usr\/bin\/dockerd/ s/$/--default-ulimit memlock=-1/' /etc/systemd/system/docker.service.d/10-machine.conf
```
akan terbentuk file override.conf pada /etc/systemd/system/docker.service.d
```
echo -e "[Service]\nLimitMEMLOCK=infinity" | SYSTEMD_EDITOR=tee systemctl edit docker.service \
&& systemctl daemon-reload && systemctl restart docker
```
```
reboot
```
### Open Port on Ubuntu
```
ufw allow 22/tcp
ufw allow 2376/tcp
ufw allow 2377/tcp
ufw allow 7946/tcp
ufw allow 7946/udp
ufw allow 4789/udp
ufw reload
```
### Open Port on CentOS
```
firewall-cmd --add-port=22/tcp --permanent
firewall-cmd --add-port=2376/tcp --permanent
firewall-cmd --add-port=2377/tcp --permanent
firewall-cmd --add-port=7946/tcp --permanent
firewall-cmd --add-port=7946/udp --permanent
firewall-cmd --add-port=4789/udp --permanent
firewall-cmd --reload
```

### Set up swarm mode
```
docker swarm init
```
add manager nodes (optional)
```
docker swarm join-token manager
```
Add worker nodes (optional)
```
docker swarm join-token worker
```
check
```
docker node ls
```
