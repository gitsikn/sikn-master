### Update RHV Host
* Shutdown seluruh VM yang terdapat pada host
* Remove rhvm-appliance package
```
yum remove rhvm-appliance
```
* update host
From the RHV-M GUI, put the host into maintenance, then click 'Installation -> Upgrade'

> Note: If your hypervisor is a RHEL server, you can put it into > > maintenance and and upgrade via yum: ie:

```
yum -y update
shutdown -r now
```
## Download log dari rhv
```
ovirt-log-collector
sos-collector
```
