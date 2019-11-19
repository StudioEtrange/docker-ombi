# docker ombi by StudioEtrange

* Run ombi inside a docker container built upon debian
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

Available tag for studioetrange/docker-ombi:__TAG__

	latest, v3.0.4892, v3.0.4887, v3.0.4880, v3.0.4817, v3.0.4680, v3.0.4659, v3.0.4654, v3.0.4256, v3.0.4248, v3.0.4119, v3.0.4036, v3.0.3988, v3.0.3945, v3.0.3923, v3.0.3919, v3.0.3795, v3.0.3786, v3.0.3776, v3.0.3587, v3.0.3477, v3.0.3421, v3.0.3407, v3.0.3383, v3.0.3368, v3.0.3346, v3.0.3330, v3.0.3304, v3.0.3293, v3.0.3268, v3.0.3239, v3.0.3185, v3.0.3173, v3.0.3164, v3.0.3111, v3.0.3030, v3.0.3020, v3.0.3000, v3.0.2970, v2.2.1, v2.2.0, v2.1.0, v2.0.1, v1.10.1, v1.10.0, v1.9.7, v1.9.6, v1.9.5, v1.9.4, v1.9.3, v1.9.2, v1.9.1, v1.9.0, v1.8.4, v1.8.3, v1.8.2, v1.8.1, v1.8.0, v1.7.5, v1.7.4, v1.7.3, v1.7.2, v1.7.1, v1.7.0, v1.6.1, v1.6.0, v1.5.2, v1.5.1, v1.5.0, v1.4.1, v1.4.0, v1.3.0, v1.2.1, v1.2.0, v1.1

Current latest tag is version __v3.0.4892__

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
