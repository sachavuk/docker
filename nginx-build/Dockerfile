FROM centos:centos6

RUN rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm 

RUN yum -y update && yum -y groupinstall "Development Tools"

RUN mkdir /home/nginx-build

WORKDIR /home/nginx-build

RUN yum -y install lua-devel.x86_64 lua wget

RUN git clone https://github.com/simpl/ngx_devel_kit.git

RUN git clone https://github.com/openresty/lua-nginx-module.git

RUN wget http://nginx.org/download/nginx-1.6.1.tar.gz




