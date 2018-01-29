FROM ubuntu:14.04


RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
#ENV LANGUAGE en_US:en
#ENV LC_ALL en_US.UTF-8


RUN apt-get update && apt-get install -y python3 python3-scipy python3-pip python3-dev libpq-dev python3-dev libffi-dev
RUN pip3 install --upgrade pip==8.1.2
RUN ln -s /usr/bin/python3 /usr/bin/python
RUN pip install awscli

CMD mkdir -p /opt/app
VOLUME /opt/
VOLUME /usr/src/app/

ADD requirements.txt /opt/app/requirements.txt
RUN pip install -r /opt/app/requirements.txt
# RUN
# CMD
RUN pip install scipy --upgrade


MAINTAINER Guillaume Carre <guillaume.carre@gmail.com>

RUN apt-get update
RUN apt-get install -y software-properties-common

# Install Java
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
RUN apt-get install -q -y oracle-java8-installer
RUN apt-get install -q -y oracle-java8-set-default

CMD ["java"]
