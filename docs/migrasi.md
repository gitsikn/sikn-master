### Prinsip Migrasi Database
a. backup database   
b. drop database lama    
c. create database baru  
d. restore database   


####
Install rsync (centos)
```
yum install rsync
```
### Backup database sql
**Lokasi: Server Lama** 

> Gunakan screen jika ukuran database yang dibackup besar
```

a. memulai screen dengan nama
screen -S nama_screen

b. keluar screen
ctrl + a + d

c. masuk screen
screen -r nama_screen

d. daftar screen
screen -ls
```
### Dump SQL Data
```
mysqldump -u root -p old_database > /tmp/nama_simpul.sql
(input password)
```


### download data sql
**Lokasi: Server Baru**
```
mkdir -p /backup_data/backup_sql \
&& cd /backup_data 

rsync -r -v --progress -e ssh username@ip_public:/tmp/nama_simpul.sql /backup_data/backup_sql/
```
### melihat ukuran objek digital 
```
du -sch /directory_path
```

### download objek digital

```
mkdir -p /backup_data/objek-digital \
&& rsync -r -v --progress -e ssh username@ip_public:/usr/share/nginx/nama_simpul/uploads /backup_data/objek-digital
```
```
cp -rf /backup_data/nama_simpul/objek-digital/uploads /sikn/nama_simpul/
chown -R www-data:www-data /sikn/nama_simpul/uploads  
chmod -R 0755 /sikn/nama_simpul/uploads

```

### download logo
```
rsync -r -v --progress -e ssh anri@demo.sikn.go.id:/usr/share/nginx/nama_simpul/images/logo.png /backup_data/
input password
```
```
cp -rf /backup_data/logo.png /nama_simpul/images/ && chmod 644 /nama_simpul/images/logo.png
```

### download favicon
```
rsync -r -v --progress -e ssh anri@demo.sikn.go.id:/usr/share/nginx/nama_simpul/favicon.ico /backup_data/
input password
```
```
cp -rf /backup_data/favicon.ico /nama_simpul/ && chmod 644 /nama_simpul/favicon.ico

```
### Hapus Database
1. hapus database (seluruh tabel dan data)
dieksekusi pada container database
docker exec -it container_id mysql -u root -p -e 'drop database nama_database;'
input password
contoh:
```
docker exec -it 600be7eefdcb mysql -u root -p -e 'drop database database_name;'
```
atau 

masuk ke container database 
```
mysql -u root -p -e 'drop database database_name;'
```
2. membuat database (isi dengan tabel+data) bersumber dari backup sql
```
mysql -u root -p -e 'create database new_database character set utf8 collate utf8_unicode_ci;'
```
contoh:
```
docker exec -it 600be7eefdcb mysql -u root -p -e 'create database atom character set utf8 collate utf8_unicode_ci;'
```
3. copy file dari host ke container
```
docker cp /backup_data/backup_sql/database.sql container_id:/tmp/
```

### 4. Restore Database
masuk ke container
```
docker exec -it  container_id /bin/bash
```
```
mysql -u root -p new_database < /tmp/database.sql;
```
contoh:
```
mysql -u root -p atom < /tmp/karangasem.sql
```


### 5. upgrade-sql

Lokasi: Container atom
```
docker exec -it  container_id php symfony tools:upgrade-sql
```
atau 

masuk ke container AtoM
```
php symfony tools:upgrade-sql
```
### 6. index
```
docker exec -it  a4b8718b39fa php symfony cc
docker exec -it  a4b8718b39fa php symfony search:populate
```
atau masuk ke kontainer AtoM
```
php symfony cc \
&& php symfony search:populate
```

untuk kebutuhan test create admin akun
```
docker exec -it container_id php symfony tools:add-superuser --email="admin@sikn.go.id" --password="******" adminsikn
```
Hapus user 
```
php symfony tools:delete-user -f -n adminsikn
```
ukuran header plugins/arDominionPlugin/css/main.css
empat bahasa default
default culture
default time

cek bahasa
cek worker



```