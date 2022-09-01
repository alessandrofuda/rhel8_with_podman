# FROM registry.access.redhat.com/ubi8/ubi:8.4
FROM roboxes/rhel8

# https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/assembly_starting-with-containers_building-running-and-managing-containers

ARG RH_USERNAME
ARG RH_PASSWORD
ARG CONTAINER_USER
ARG CONTAINER_PSWD

RUN subscription-manager register --username ${RH_USERNAME} --password ${RH_PASSWORD} --auto-attach

RUN yum -y update \
    && yum -y upgrade \
    && yum module install -y container-tools


RUN useradd -c "${CONTAINER_USER}" ${CONTAINER_USER} \
    && echo ${CONTAINER_USER}:${CONTAINER_PSWD} | chpasswd



#RUN useradd podman \
#    && echo podman:10000:5000 > /etc/subuid \
#    && echo podman:10000:5000 > /etc/subgid

# Add non root user / group
# RUN groupadd -g 1000 www
# RUN useradd -u 1000 -ms /bin/bash -g www www

# install nginx container like reverse proxy / access point from browser

# RUN yum --disableplugin=subscription-manager -y module enable php:7.3 \
#  && yum --disableplugin=subscription-manager -y install httpd php \
#  && yum --disableplugin=subscription-manager clean all
#
#ADD index.php /var/www/html

#RUN sed -i 's/Listen 80/Listen 8080/' /etc/httpd/conf/httpd.conf \
#  && sed -i 's/listen.acl_users = apache,nginx/listen.acl_users =/' /etc/php-fpm.d/www.conf \
#  && mkdir /run/php-fpm \
#  && chgrp -R 0 /var/log/httpd /var/run/httpd /run/php-fpm \
#  && chmod -R g=u /var/log/httpd /var/run/httpd /run/php-fpm

EXPOSE 9090
# USER www
# USER podman

# CMD php-fpm & httpd -D FOREGROUND
CMD /bin/bash
