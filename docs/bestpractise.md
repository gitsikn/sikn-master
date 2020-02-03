
konek ke vpn

Login ke server/portainer dimana aplikasi akan dideploy:   
a. worker1  
b. worker2  
c. worker3  
d. worker4  
e. worker5  
f. worker6   
```
cd /sikn \
&& sudo git clone -b stable/2.5.x https://github.com/gitsikn/atom.git nama_simpul
```
### Versi qa 2.6.x
```
cd /sikn \
git clone -b qa/2.6.x https://github.com/artefactual/atom.git nama_simpul
```

```
sudo chmod -R 777 /sikn/nama_simpul/percona_data \
&& sudo chmod -R 777 /sikn/nama_simpul/elasticsearch_data
```

referensi docker-stack
-------------------------------
> https://github.com/gitsikn/atom/blob/stable/2.5.x/docker/docker-stack.yml

> sesuaikan nama_simpul dengan nama simpul yang akan dideploy

1. Masuk kedalam Portainer
membuat stack dengan nama simpul

deploy

jalankan di container "atom"

```
make -C plugins/arDominionPlugin \
&& php symfony tools:purge --demo
```

publish port (jangan bentrok)

test di browser
http://192.168.101.100:63106

https://github.com/gitsikn/docker_documentation/wiki/6.-Migrasi-Data

### Membuat database baru

> Lokasi Eksekusi: Container Percona (database)
```
mysql -u root -p -e 'drop database nama_simpul;'
mysql -u root -p -e 'create database nama_simpul character set utf8 collate utf8_unicode_ci;'
```
### Download data
mkdir -p /sikn/backup_data/nama_simpul  
```
rsync -r -v --progress -e ssh anri@103.28.21.27:/tmp/nama_simpul.sql /sikn/backup_data/nama_simpul
```
atau jika menggunakan port tertentu
```
rsync -r -v --progress -e 'ssh -p 221' anri@103.28.21.27:/tmp/nama_simpul.sql /sikn/backup_data/nama_simpul
```
### Copy data dari direktory backup ke lokasi database
```
cp -rf /sikn/backup_data/nama_simpul/nama_simpul.sql /sikn/nama_simpul/percona_data

```

### Restore Database
> lokasi: Container Database
```
mysql -u root -p nama_simpul < /var/lib/mysql/nama_simpul.sql;
```
### Upgrade Database

> lokasi: container atom
```
php symfony tools:upgrade-sql
```
```
php symfony cc \
&& php symfony search:populate
```
### Download Objek Digital

```
mkdir -p /sikn/backup_data/nama_simpul/ \
cd /sikn/backup_data/nama_simpul \
&& rsync -r -v --progress -e ssh anri@103.28.21.27:/usr/share/nginx/nama_simpul/uploads /sikn/backup_data/nama_simpul/
```
```
cp -rf /sikn/backup_data/nama_simpul/uploads /sikn/nama_simpul/ \
&& chmod -R 0755 /sikn/nama_simpul/uploads
```
### Logo dan Favicon
```
rsync -r -v --progress -e ssh anri@103.28.21.27:/usr/share/nginx/nama_simpul/images/logo.png /sikn/backup_data/nama_simpul
```
```
rsync -r -v --progress -e ssh anri@103.28.21.27:/usr/share/nginx/nama_simpul/favicon.ico /sikn/backup_data/nama_simpul
```
```
cp -rf /sikn/backup_data/nama_simpul/favicon.ico /sikn/nama_simpul/ \
&& chmod -R 0755 /sikn/nama_simpul/favicon.ico

cp -rf /sikn/backup_data/nama_simpul/logo.png /sikn/nama_simpul/ \
&& chmod -R 0755 /sikn/nama_simpul/images/logo.png
