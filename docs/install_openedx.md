# Referensi   
https://edx.readthedocs.io/projects/edx-installing-configuring-and-running/en/latest/installation/install_devstack.html

Prerequisite:  
a. docker    
b. docker-compose  
c. make  

# Install Devstack
```
mkdir /openedx \
&& cd /openedx \
&& git clone https://github.com/edx/devstack \
&& cd devstack
```
```
git checkout open-release/ironwood.master
export OPENEDX_RELEASE=ironwood.master
make dev.checkout
make dev.clone
make dev.provision
```
### Start openedx service
`
make dev.up
`
### URL openedx
| Services            |                URL                |
|---------------------|:---------------------------------:|
| Catalog/Discovery   |  http://localhost:18381/api-docs/ |
| Credentials         |   http://localhost:18150/api/v2/  |
| E-Commerce/Otto     | http://localhost:18130/dashboard/ |
| LMS                 | http://localhost:18000/           |
| Notes/edx-notes-api | http://localhost:18120/api/v1/    |
| Studio/CMS          | http://localhost:18010/           |

### Update Latest Image 
```
make down     
make pull     
make dev.up    
```
