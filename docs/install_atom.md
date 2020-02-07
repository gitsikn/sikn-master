### 4. Install AtoM
**Catatan: Instalasi pada simpul resource tidak dibatasi**
versi 2.5

sesuaikan **nama_simpul**, konfigurasi pada file **sikn.yml** di docker-yml/sikn.yml
find and replace nama_simpul dengan nama simpul baru (alt + r) pada nano editor  
pastikan port pada service nginx belum digunakan sebelumnya
```
mkdir -p /sikn \
&& cd /sikn \
&& git clone -b stable/2.5.x https://github.com/gitsikn/atom.git nama_simpul && cd /sikn/nama_simpul/docker/
```

```
chmod -R 755 /nama_simpul
```
```
sudo chmod -R 777 /sikn/nama_simpul/percona_data \
&& sudo chmod -R 777 /sikn/nama_simpul/elasticsearch_data
```
sesuikan parameter database pada file docker-stack.yml

password dapat digenerate menggunakan tools keypass
```
...
#percona_database
  percona:
    image: percona:5.6
    environment:
      - MYSQL_ROOT_PASSWORD=changethis
      - MYSQL_DATABASE=nama_simpul
      - MYSQL_USER=nama_simpul
      - MYSQL_PASSWORD=changethis
...
```

### 5. membuat env variable dan menjalankan container (optional)
```
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml"
```
atau dieksekusi dengan pilihan -f lokasi_file
```
cd nama_simpul 
docker-compose -f docker-compose.yml up -d
```
### mode docker swarm 
```
docker stack deploy -c docker-stack.yml nama_simpul
```

periksa seluruh container hingga seluruh container dengan status running 
```
docker stack ls
docker stack ps nama_stack 
docker service logs nama_service
```
```
docker ps -a 
```
### 6. build stylesheets
```
docker ps 
docker-compose -f docker-compose-file.yml exec service_name make -C plugins/arDominionPlugin
```
atau
```
docker exec container_id make -C plugins/arDominionPlugin
```
atau  
masuk ke docker gui (portainer) dan masuk ke console AtoM
```
make -C plugins/arDominionPlugin
```


### 7. purge/hapus data dari database atom dengan mengunakan mode demo 

```
docker-compose exec atom php symfony tools:purge --demo
php symfony cc && php symfony search:populate
```
atau 
masuk ke docker gui (portainer) dan masuk ke console AtoM
```
php symfony tools:purge --demo
```

10. restart container worker
```
docker restart $(docker ps -a -q)
```

melihat seluruh container dengan status up/exited

docker ps -a

mendapatkan container id 
docker ps -aq


### 11. test AtoM
tambahkan dns lokal pada file C:\Windows\System32\drivers\etc\host

sikn.nama_simpul.go.id
 