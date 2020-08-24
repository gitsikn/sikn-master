### Membuat domain 

add domain below to local DNS or public DNS  

dashboard.domain_name  
alertmanager.domain_name  
unsee.domain_name  
prometheus.domain_name  

```
export DOMAIN=root_domain.go.id &&
export ADMIN_USER=admin &&
export ADMIN_PASSWORD=password &&
export HASHED_PASSWORD=$(openssl passwd -apr1 $ADMIN_PASSWORD) &&
export TRAEFIK_PUBLIC_TAG=traefik-public
```

```
CentOS  
yum install git

Ubuntu  
apt install git 
```
```

cd /opt/ \
&& git clone https://github.com/gitsikn/swarmprom.git \
&& cd swarmprom
```
**Multiple node** 
```
docker stack deploy -c docker-compose.traefik.yml swarmprom
```
**Single node** 
```
docker-compose -f docker-compose.traefik.yml up -d
```
LOKASI: CONTAINER PERCONA
Open port untuk grafana (3306), dengan menggunakan portainer

membuat user database dengan hak akses ALL PRIVILEGES (CREATE, DROP, DELETE, INSERT, SELECT, UPDATE) untuk grafana (pada container database)
```
GRANT ALL PRIVILEGES ON database_name.* TO 'grafanaReader'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
```
membuat user database dengan hak akses read untuk grafana (pada container database)
```
mysql> GRANT SELECT ON DATABASE_NAME.* TO 'grafanareader'@'%' IDENTIFIED BY 'PASSWORD';
```
Melihat user database
```
SELECT user, host FROM mysql.user;
```
Menampilkan hak pada user database
```
SHOW GRANTS FOR 'USER_NAME'@'IP_ADDRESS';
```
Revoke hak akses
```
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'USER_NAME'@'IP_ADDRESS';
```
DROP USER
```
DROP USER 'bloguser'@'localhost';
```