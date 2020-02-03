###Backup Percona Database
Referensi:  
https://www.percona.com/doc/percona-xtrabackup/2.2/xtrabackup_bin/creating_a_backup.html

Proses backup menggunakan tools xtrabackup
lokasi database pada percona: /var/lib/mysql

## Install xtrabackup
```
yum install xtrabackup -y
```

```
xtrabackup --backup --datadir=/var/lib/mysql/ --target-dir=/data/backups/mysql/
```


### Restore Percona Database
Referensi:  
https://www.percona.com/doc/percona-xtrabackup/2.1/xtrabackup_bin/restoring_a_backup.html
