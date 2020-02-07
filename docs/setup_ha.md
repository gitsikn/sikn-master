https://medium.com/running-a-software-factory/setup-3-node-high-availability-cluster-with-glusterfs-and-docker-swarm-b4ff80c6b5c3

http://embaby.com/blog/using-glusterfs-docker-swarm-cluster/  
https://www.linuxtechi.com/setup-glusterfs-storage-on-centos-7-rhel-7/


buat disk (shareable)
attach to vm 

format disk (lsblk)  

**Edit /etc/hosts**

```
(gluster1)# cat /etc/hosts
127.0.0.1       localhost
192.168.101.101   gluster1
192.168.101.102   gluster2
192.168.101.103   gluster3
```
```
(gluster2)# cat /etc/hosts
127.0.0.1       localhost
192.168.101.101   gluster1
192.168.101.102   gluster2
192.168.101.103   gluster3
```
```
(gluster3)# cat /etc/hosts
127.0.0.1       localhost
192.168.101.101   gluster1
192.168.101.102   gluster2
192.168.101.103   gluster3
```

### Gluster 1
```
(gluster1)# mkdir -p /gluster/bricks/1
(gluster1)# mkfs.xfs /dev/sdb
(gluster1)# echo '/dev/sdb /gluster/bricks/1 xfs defaults 0 0' >> /etc/fstab
(gluster1)# mount -a
(gluster1)# mkdir /gluster/bricks/1/brick
(gluster1)# lsblk
```
### Gluster 2
```
(gluster2)# mkdir -p /gluster/bricks/2
(gluster2)# mkfs.xfs /dev/sdb
(gluster2)# echo '/dev/sdb /gluster/bricks/2 xfs defaults 0 0' >> /etc/fstab
(gluster2)# mount -a
(gluster2)# mkdir /gluster/bricks/2/brick
(gluster2)# lsblk
```` 
### Gluster 3
```
(gluster3)# mkdir -p /gluster/bricks/3
(gluster3)# mkfs.xfs /dev/sdb
(gluster3)# echo '/dev/sdb /gluster/bricks/3 xfs defaults 0 0' >> /etc/fstab
(gluster3)# mount -a
(gluster3)# mkdir /gluster/bricks/3/brick
(gluster3)# lsblk
```` 
> Lakukan hal yang sama apabila ada penambahan gluster


### Install GlusterFS
### CentOS
```
yum install wget -y \
&& yum install centos-release-gluster -y \
&& yum install epel-release -y \
&& yum install glusterfs-server -y \
&& systemctl start glusterd \
&& systemctl enable glusterd \
&& systemctl status glusterd
```
### Open Firewall
```
firewall-cmd --zone=public --add-port=24007-24008/tcp --permanent
firewall-cmd --zone=public --add-port=24009/tcp --permanent
firewall-cmd --zone=public --add-service=nfs --add-service=samba --add-service=samba-client --permanent
firewall-cmd --zone=public --add-port=111/tcp --add-port=139/tcp --add-port=445/tcp --add-port=965/tcp --add-port=2049/tcp --add-port=38465-38469/tcp --add-port=631/tcp --add-port=111/udp --add-port=963/udp --add-port=49152-49251/tcp --permanent
firewall-cmd --reload
```
### Peer with other Gluster VMs
```
(gluster1)# gluster peer probe gluster2
(gluster1)# gluster peer probe gluster3
(gluster1)# gluster peer status
```
### Setup the Gluster “replicated volume”
```
(gluster1)# gluster volume create gfs \
replica 3 \
gluster1:/gluster/bricks/1/brick \
gluster2:/gluster/bricks/2/brick \
gluster3:/gluster/bricks/3/brick
```
```
(gluster1)# gluster volume start gfs
(gluster1)# gluster volume status gfs
```
### Setup security and authentication for this volume
```
(gluster1)# gluster volume set gfs auth.allow 192.168.101.101,192.168.101.102,192.168.101.103
```
### Mount the glusterFS volume where applications can access the files
```
(gluster1)# echo 'localhost:/gfs /sikn glusterfs defaults,_netdev,backupvolfile-server=localhost 0 0' >> /etc/fstab
(gluster1)# mount.glusterfs localhost:/gfs /sikn

(gluster2)# echo 'localhost:/gfs /sikn glusterfs defaults,_netdev,backupvolfile-server=localhost 0 0' >> /etc/fstab
(gluster2)# mount.glusterfs localhost:/gfs /sikn

(gluster3)# echo 'localhost:/gfs /sikn glusterfs defaults,_netdev,backupvolfile-server=localhost 0 0' >> /etc/fstab
(gluster3)# mount.glusterfs localhost:/gfs /sikn
```
### Verify
```
df -Th
```
