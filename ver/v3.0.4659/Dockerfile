FROM debian:stretch
LABEL maintener "StudioEtrange <nomorgan@gmail.com>"
LABEL description "Ombi on docker"

# PARAMETERS -------------------------------------------------------

# Service generic parameters
ENV SERVICE_NAME ombi
ENV SERVICE_VERSION v3.0.4659
# User id which will launch the service start command (NOTE : supervisord itself is run with root)
ENV SERVICE_USER 0
ENV SERVICE_PORT 5000
ENV SERVICE_INSTALL_DIR /opt/${SERVICE_NAME}

# Specific service parameters
# path to store service data and configuration
ENV SERVICE_DATA_PATH /data/${SERVICE_NAME}
# external paths used by service
ENV SERVICE_VOLUME_PATH ${SERVICE_DATA_PATH}
# args used by supervisor context for running service
ENV SERVICE_EXPORT_ARG SERVICE_DATA_PATH SERVICE_PORT SERVICE_INSTALL_DIR

# System parameters
ENV PYTHON_MAJOR_VERSION 2
ENV PYTHON_VERSION 2.7.15
ENV MINICONDA_VERSION 4.5.11
ENV CONDA_ENV ${SERVICE_NAME}
ENV PATH /opt/miniconda/bin:$PATH
ENV SUPERVISOR_VERSION 4.0.3

# TREE FILESYSTEM --------------------------------------------------
RUN mkdir -p /etc/supervisor/conf.d && mkdir -p /var/log/supervisor && mkdir -p "${SERVICE_INSTALL_DIR}"

WORKDIR "${SERVICE_INSTALL_DIR}"



# COMPONENTS -------------------------------------------------------

# debian packages
RUN echo "deb http://httpredir.debian.org/debian stretch main non-free" >> /etc/apt/sources.list \
	&& echo "deb http://httpredir.debian.org/debian stretch-updates main non-free" >> /etc/apt/sources.list \
	&& echo "deb http://httpredir.debian.org/debian stretch-backports main non-free" >> /etc/apt/sources.list

RUN apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
					    ca-certificates curl wget openssl libssl-dev bzip2 unrar-free locales \
					    libicu-dev libunwind8 libcurl4-openssl-dev \
	&& rm -rf /var/lib/apt/lists/*

# locales
# https://stackoverflow.com/a/38553499
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8

ENV LANG en_US.UTF-8 

# Python
RUN MINICONDA_FILE=Miniconda${PYTHON_MAJOR_VERSION}-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    wget https://repo.continuum.io/miniconda/${MINICONDA_FILE} -O /opt/${MINICONDA_FILE} && \
    chmod +x /opt/$MINICONDA_FILE && \
    /opt/$MINICONDA_FILE -b -p /opt/miniconda && \
    rm /opt/$MINICONDA_FILE

# create dedicated environment
RUN /opt/miniconda/bin/conda create -y -n ${CONDA_ENV} python=${PYTHON_VERSION}

# install supervisor
RUN bash -c "source activate ${CONDA_ENV} && \
                pip install supervisor==${SUPERVISOR_VERSION}"

# SERVICE INSTALL -------------------------------------------------------
# NOTE : asset url changed at some point from linux.tar.gz to linux-x64.tar.gz
RUN selected_version="$(curl -s https://api.github.com/repos/Ombi-app/Ombi/releases/tags/${SERVICE_VERSION} | grep linux-x64.tar.gz | grep browser_download_url  | head -1 | cut -d \" -f 4)" && \
	if [ "${selected_version}" = "" ]; then selected_version="$(curl -s https://api.github.com/repos/Ombi-app/Ombi/releases/tags/${SERVICE_VERSION} | grep linux.tar.gz | grep browser_download_url  | head -1 | cut -d \" -f 4)"; fi && \
	curl -k -SL "${selected_version}" \
	| tar -xzf - -C ${SERVICE_INSTALL_DIR}

# SUPERVISOR -------------------------------------------------------
# add supervisor default configuration
COPY files/supervisord.conf /etc/supervisor/supervisord.conf
# add supervisor service configuration
COPY files/supervisord-${SERVICE_NAME}.conf /etc/supervisor/conf.d/supervisord-${SERVICE_NAME}.conf


# DOCKER RUN PARAMETERS ----------------------------------------------
# will contain service data and configuration
VOLUME /data


# supervisor web interface
EXPOSE 9999

# service port
EXPOSE ${SERVICE_PORT}

# entrypoint
COPY files/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["docker-entrypoint.sh"]

# default command
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]
