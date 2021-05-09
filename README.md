# docker ombi by StudioEtrange

* Run [ombi](https://github.com/Ombi-app/Ombi) inside a docker container built upon debian
* Based on ombi github repository
* Choice of ombi version
* Use supervisor to manage ombi process
* Can choose a specific unix user to run ombi inside docker
* By default ombi configuration files will be in a folder named 'ombi' which will be contained in a docker volume /data


## Quick Usage

for running latest stable version of ombi :

	mkdir -p data
	docker run --name ombi -d -v $(pwd)/data:/data -p 5000:5000 studioetrange/docker-ombi

then go to http://localhost:5000

## Docker tags

Pre-setted buildable docker tags for studioetrange/docker-ombi:__TAG__

	latest, v4.0.1351, v4.0.1350, v4.0.1349, v4.0.1348, v4.0.1347, v4.0.1345, v4.0.1344, v4.0.1342, v4.0.1340, v4.0.1339, v4.0.1338, v4.0.1336, v4.0.1334, v4.0.1333, v4.0.1332, v4.0.1329, v4.0.1328, v4.0.1324, v4.0.1322, v4.0.1319, v4.0.1314, v4.0.1313, v4.0.1309, v4.0.1299, v4.0.1292, v4.0.1290, v4.0.1286, v4.0.1282, v4.0.1281, v4.0.1277, v4.0.1275, v4.0.1262, v4.0.1261, v4.0.1260, v4.0.1259, v4.0.1257, v4.0.1256, v4.0.1255, v4.0.1222, v4.0.1204, v4.0.1203, v4.0.1156, v4.0.1155, v4.0.1154, v4.0.1153, v4.0.1152, v4.0.1151, v4.0.1150, v4.0.1142, v4.0.1139, v4.0.1136, v4.0.1135, v4.0.1134, v4.0.1133, v4.0.1132, v4.0.1131, v4.0.1128, v4.0.1122, v4.0.1120, v4.0.1119, v4.0.1118, v4.0.1117, v4.0.1116, v4.0.1112, v4.0.1104, v4.0.1103, v4.0.1102, v4.0.1095, v4.0.1090, v4.0.1089, v4.0.1085, v4.0.1080, v4.0.1078, v4.0.1067, v4.0.1062, v4.0.1040, v4.0.1039, v4.0.1037, v4.0.1036, v4.0.1035, v4.0.1032, v4.0.1014, v4.0.1013, v4.0.1012, v4.0.1011, v4.0.1009, v4.0.999, v4.0.994, v3.0.5223, v3.0.5202, v3.0.4892, v3.0.4887, v3.0.4880, v3.0.4817, v3.0.4680, v3.0.4659, v3.0.4654, v3.0.4256, v3.0.4248, v3.0.4119

Current latest tag is version __v4.0.1351__

## Instruction

### Build from github source

	git pull https://github.com/StudioEtrange/docker-ombi
	cd docker-ombi
	docker build -t=studioetrange/docker-ombi .

### Retrieve image from docker registry

	docker pull studioetrange/docker-ombi

### Standard usage

	mkdir -p data
	docker run --name ombi -d -v $(pwd)/data:/data -p 5000:5000 -e SERVICE_USER=$(id -u):$(id -g) -v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime:ro studioetrange/docker-ombi

### Full run parameters

	docker run --name ombi -d -v <data path>:/data -p <ombi http port>:5000 -e SERVICE_USER=<uid[:gid]>  -p <supervisor manager http port>:9999 -v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime:ro studioetrange/docker-ombi:<version>

### Volumes

Inside container
`/data/ombi` will contain ombi configuration and files

If the path of this volume do not exist on the host while your are mounting them inside container, docker will create it automaticly with root user. You should use mkdir before launching docker to control ownership.

### Access to a running instance

supervisorctl access

	docker exec -it ombi bash -c ". activate ombi && supervisorctl"
	
bash access

	docker exec -it ombi bash -c ". activate ombi"
	
### Test a shell inside a new container without ombi running

	docker run -it --rm studioetrange/docker-ombi bash
	
### Stop and destroy all previously launched services

	docker stop ombi && docker rm ombi
	
## Access point

### ombi

	Go to http://localhost:OMBI_HTTP_PORT/

### Supervisor process manager

	Go to http://localhost:SUPERVISOR_HTTP_WEB/

## Notes on Github / Docker Hub Repository

* This github repository is linked to an automated build in docker hub registry.

	https://registry.hub.docker.com/u/studioetrange/docker-ombi/

* _update.sh_ is only an admin script for this project which update and add new versions. This script do not auto create missing tag in docker hub webui. It is only for this project admin/owner purpose.
