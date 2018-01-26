FROM java:8

RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main multiverse restricted universe \
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main multiverse restricted universe \
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main multiverse restricted universe \
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main multiverse restricted universe \
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main multiverse restricted universe \
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main multiverse restricted universe \
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main multiverse restricted universe \
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main multiverse restricted universe \
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main multiverse restricted universe \
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main multiverse restricted universe" >> /etc/apt/source.list

RUN apt-get update
