### Harvest data dari simpul ke JIKN
Tentukan Simpul yang akan di harvest 

1. login ke server sumber data (ssh)
2. masuk ke direktori simpul (/usr/share/nginx/nama_simpul)
3. jalankan perintah export (arsip terpublikasi)

## Export Data 
### Format XML 
```
php symfony export:bulk --public --items-until-update=5 --single-slug="nama_slug" /home/anri/XML_FOR_JIKN/nama_deskripsi.xml
```
### Format CSV
```
php symfony csv:export --public --items-until-update=5 --single-slug="arsip-statis-direktorat-pembangunan-desa-ditbangdes" /home/anri/XML_FOR_JIKN/arsip-statis-direktorat-pembangunan-desa-ditbangdes.csv
```
```
php symfony csv:export --single-slug="slug-name" /nama-file.csv
```

4. download file xml/csv dari server sumber data ke server tujuan
```
rsync -r -v --progress -e ssh anri@103.28.21.27:/home/anri/XML_FOR_JIKN/nama_deskripsi.xml /backup_data/
```

input password

4. copy file xml dari host ke atom container
```
docker cp /backup_data/nama_deskripsi.xml 7db0f944fd2b:/jikn/deskripsi_arsip.xml
```

5. import file xml ke atom

Format XML
```
php symfony import:bulk --output="/tmp/output-results.csv" /jikn 
```
Format CSV
```
php symfony csv:import /jikn/nama_deskripsi.csv
```
