docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname

docker push dockersikn/grafana:tagname
docker push dockersikn/grafana:tagname

contoh: 
account docker hub: dockersikn
buat repository: misal (grafana)
maka akan terbentuk repository kosong dengan nama lokasi 
dockersikn/grafana

build docker image
docker build /path/to/Dockerfile -t dockerhubaccount/repository:tagname
contoh:
docker build /opt/grafana/ -t dockersikn/grafana:latest

maka untuk push image ke docker hub dengan syntax
docker push dockersikn/grafana:6.2.5

