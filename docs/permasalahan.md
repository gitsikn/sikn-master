### NFS in centos  

https://www.howtoforge.com/nfs-server-and-client-on-centos-7


https://gist.github.com/ruanbekker/4a9c0d250bce9f84482f2a788ce92131#file-docker-nfs-volumes-md    
https://blog.dahanne.net/2017/11/20/docker-swarm-and-nfs-volumes/  
https://sysadmins.co.za/docker-swarm-persistent-storage-with-nfs/  
http://rockstor.com/docs/install.html  
https://github.com/ContainX/docker-volume-netshare  
https://www.portainer.io/2018/07/use-nfs-docker-local%E2%80%8B-volume-driver-portainer-io/  



membuat nfs di centos
---------------------
SERVER SIDE
---------------------
1. install packages:
```
yum install nfs-utils
```
2. membuat directory yang akan dishare melalui NFS
```
mkdir /var/nfsshare
```
3. atur permisi dari directory /var/nfsshare
```
chmod -R 777 /var/nfsshare
chown nfsnobody:nfsnobody /var/nfsshare
```
4. start and enable service
```
systemctl enable rpcbind \
&& systemctl enable nfs-server \
&& systemctl enable nfs-lock \
&& systemctl enable nfs-idmap \
&& systemctl start rpcbind \
&& systemctl start nfs-server \
&& systemctl start nfs-lock \
&& systemctl start nfs-idmap
```
5. membuat file yang berisi konfigurasi informasi client dan hak yang diberikan 
```
nano /etc/exports
```
```
/var/nfsshare    192.168.101.101(rw,sync,no_root_squash,no_all_squash)
```
catatan: 192.168.101.101 adalah alamat client yang berhak mengakses dir nfs, 
dapat menggunakan * agar dir nfs bisa diakses oleh client tidak terdaftar

6. restart service nfs
```
systemctl restart nfs-server
```
7. pengaturan service nfs di firewall
```
firewall-cmd --permanent --zone=public --add-service=nfs
firewall-cmd --permanent --zone=public --add-service=mountd
firewall-cmd --permanent --zone=public --add-service=rpc-bind
firewall-cmd --reload
```
---------------------
CLIENT SIDE
---------------------
CentOS
1. Install the nfs-utild package
```
yum install nfs-utils
```
Ubuntu
```
sudo apt-get install nfs-common
```
2. membuat NFS directory mounts points
```
mkdir -p /mnt/docker_volume  
sudo chown nobody:nogroup /mnt/docker_volume
```
3. mount nfs ke directory yg sudah dibuat pada sisi client 
```
mount -t nfs 192.168.101.30:export/container /mnt/docker_volume
```
4. cek nfs 
```
df -kh
```
5. permanent nsf mounting
```
nano /etc/fstab
```
```
[...]
192.168.101.30:export/container    /mnt/docker_volume   nfs defaults 0 0
```


https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html