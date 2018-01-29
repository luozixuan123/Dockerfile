FROM phusion/baseimage:0.9.18
# debian based; https://github.com/docker-library/openjdk/blob/master/openjdk-8-jre/Dockerfile

# we need python- for aws-cli, if nothing else.
ADD get-pip.py /usr/src/get-pip.py
ADD requirements.txt /usr/src/requirements.txt

## OpenJDK 8 PPA
RUN echo deb http://ppa.launchpad.net/openjdk-r/ppa/ubuntu trusty main > /etc/apt/sources.list.d/openjdk8.list && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys DA1A4A13543B466853BAF164EB9B1D8886F44E2A && DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y libpq-dev gcc python3-dev openjdk-8-jre-headless openjdk-8-jdk unzip libffi-dev build-essential libssl-dev
RUN /var/lib/dpkg/info/ca-certificates-java.postinst configure && apt-get install -y ca-certificates ca-certificates-java
RUN python3 /usr/src/get-pip.py && pip3 install --upgrade awscli && LANG=en_US.UTF-8 pip install --upgrade -r /usr/src/requirements.txt

ADD aws-rds-ssl-2016.pem /usr/src/aws-rds-ssl-2016.pem
# 'changeit' is the default for java. yeah, wtf.
RUN keytool -import -keystore /etc/ssl/certs/java/cacerts -file /usr/src/aws-rds-ssl-2016.pem -storepass 'changeit' -noprompt -alias aws-rds-2016-v2
# currently (12/21/2016) 174 certs. We just want to make sure it isn't zero or one.
RUN keytool -list -keystore /etc/ssl/certs/java/cacerts -storepass 'changeit' -noprompt |grep -c "Certificate fingerprint"
RUN CERTCOUNT=$(keytool -list -keystore /etc/ssl/certs/java/cacerts -storepass 'changeit' -noprompt |grep -c "Certificate fingerprint"); if [ "$CERTCOUNT" -gt 100 ]; then echo "we have enough certs ($CERTCOUNT)"; else echo "wat, we expected more certs ($CERTCOUNT)"; exit 1; fi

ENV build_s3_bucket="BUCKET_NAME"
ENV build_s3_key="BUCKET_KEY"

# RUN
CMD mkdir -p /opt/app
VOLUME /opt/app/
CMD /sbin/my_init -- bash -c "mkdir -p /opt/app && aws --region=us-east-1 s3 cp s3://${build_s3_bucket}/${build_s3_key} /opt/app/app.tgz && tar -C /opt/app/ -xzvf /opt/app/app.tgz && bash -ev /opt/app/run.sh"


# look for new versions here: https://github.com/phusion/baseimage-docker/blob/master/Changelog.md
# has 'python3', nothing linked to 'python'.

# need gcc to compile psycopg2
# env: http://askubuntu.com/a/506635/69983
ADD get-pip.py /usr/src/get-pip.py
ADD requirements.txt /usr/src/requirements.txt


# neurodebian source and key: pandas, scipy, etc.
# http://neuro.debian.net/pkglists/toc_pkgs_for_release_trusty.html
# http://neuro.debian.net/pkgs/python3-pandas-lib.html

##ADD neurodebian.sources.list /etc/apt/sources.list.d/neurodebian.sources.list
##RUN apt-key adv --recv-keys --keyserver hkp://pgp.mit.edu:80 0xA5D32F012649A5A9

# need python3-dev for any pip installs, basically. freetype, pkg-config are for matplotlib
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y libpq-dev python3-dev g++ libfreetype6-dev pkg-config libffi-dev build-essential libssl-dev # "python3-pandas=0.17.*"
## others: python3-scipy python3-pandas-lib python3-pandas

RUN python3 /usr/src/get-pip.py && pip3 install "psycopg2>=2.6,<2.7" cython "pandas>=0.18,<0.19" "scipy>=0.17.1,<0.18" awscli && LANG=en_US.UTF-8 pip install --upgrade -r /usr/src/requirements.txt && python3 -m nltk.downloader -d /usr/local/share/nltk_data wordnet punkt stopwords
#RUN DEBIAN_FRONTEND=noninteractive apt-get autoremove -y gcc
# keep this for later pip work

RUN ln -s /usr/bin/python3 /usr/bin/python

RUN mkdir -p /opt/app
VOLUME /opt/
VOLUME /usr/src/app/

ENV build_s3_bucket="BUCKET_NAME"
ENV build_s3_key="BUCKET_KEY"

# make sure volumes are the last thing done in their directory. otherwise contents are discarded with a further file operation.
VOLUME /opt/
VOLUME /usr/src/app/

# no matter the command, we wrap it with my_init, which gets the services running.
CMD /sbin/my_init -- bash -c "mkdir -p /opt/app && aws --region=us-east-1 s3 cp s3://${build_s3_bucket}/${build_s3_key} /opt/app/app.tgz && tar -C /opt/app/ -xzvf /opt/app/app.tgz && bash /opt/app/run.sh"
