FROM centos:centos6

RUN rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm 
RUN yum -y update
RUN useradd builduser 
ADD nginx /etc/init.d/nginx
RUN chmod +x /etc/init.d/nginx
RUN useradd -r nginx
RUN yum -y install lua-devel.x86_64 lua wget pcre-devel zlib-devel openssl-devel readline-devel vim unzip  libxslt-devel gd-devel mod_perl-devel GeoIP-devel tar git gcc gcc-c++ make

WORKDIR /home/builduser/

RUN wget http://nginx.org/download/nginx-1.6.1.tar.gz
RUN tar xf /home/builduser/nginx-1.6.1.tar.gz

WORKDIR /home/builduser/nginx-1.6.1
RUN mkdir modules
RUN cd modules && git clone https://github.com/simpl/ngx_devel_kit.git
RUN cd modules && git clone https://github.com/openresty/lua-nginx-module.git
RUN chown -R builduser:builduser /home/builduser

USER builduser 
RUN ./configure --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/lib/nginx/tmp/client_body --http-proxy-temp-path=/var/lib/nginx/tmp/proxy --http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi --http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi --http-scgi-temp-path=/var/lib/nginx/tmp/scgi --pid-path=/var/run/nginx.pid --lock-path=/var/lock/subsys/nginx --user=nginx --group=nginx  --with-ipv6 --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_xslt_module --with-http_image_filter_module --with-http_geoip_module --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_degradation_module --with-http_stub_status_module --with-http_perl_module --with-mail --with-mail_ssl_module --with-debug --add-module=/home/builduser/nginx-1.6.1/modules/lua-nginx-module --add-module=/home/builduser/nginx-1.6.1/modules/ngx_devel_kit 
RUN make -j2
ADD nginx.conf /etc/nginx/nginx.conf
ADD nginx.lua /etc/nginx/nginx.lua

USER root
RUN make install 
RUN mkdir -p /var/lib/nginx/tmp/client_body
RUN /etc/init.d/nginx configtest 

RUN chkconfig --level 345 nginx on 

EXPOSE 80
