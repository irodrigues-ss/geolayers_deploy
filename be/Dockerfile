FROM ubuntu:20.04

RUN apt update -y

###### Configurando UTF-8 ##########
RUN apt-get install -y locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8
ENV LANG en_US.UTF-8

###### Configurando Data e Hora ##########
ENV TZ=America/Sao_Paulo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt update && apt upgrade tzdata -y

###### Instalando o Python
RUN apt install python3.9 python3.9-dev python3.9-distutils python3-pip -y

##### Instalando GDAL
RUN apt install gdal-bin libgdal-dev -y
ENV CPLUS_INCLUDE_PATH=/usr/include/gdal
ENV C_INCLUDE_PATH=/usr/include/gdal

###### Instalando Pipenv
RUN pip3 install pipenv==2020.11.15

###### Configurando a aplicacao
ARG APP_NAME=geolayers_be
COPY ./$APP_NAME /app
WORKDIR /app
RUN pipenv install --system

CMD uvicorn --host 0.0.0.0 --port 8000 src.app:app
