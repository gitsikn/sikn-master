Referensi  
https://www.mkdocs.org/  
https://romandc.com/techtalk-mkdocs/

install Mkdocs  
Kebutuhan
Python 
PIP 
MkDocs supports Python versions 2.7, 3.4, 3.5, 3.6, 3.7 and pypy.

Enable the EPEL repository  

``
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
``
``
Install PIP 
```
sudo yum install python-pip
```

Install virtualenv
```
pip install virtualenv
```
Upgrade PIP 
```
pip install --upgrade pip
```
membuat virtual environment
```
virtualenv env
source env/bin/activate
```
generate project
```
mkdocs new my-project
cd my-project
```