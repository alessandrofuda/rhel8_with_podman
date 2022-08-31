## Guide

### Option 1
`docker run -it --name test_container_rhel84 registry.access.redhat.com/ubi8/ubi:8.4 bash`


(Run `docker run -it roboxes/rhel8`  - It run rhel8 container and go inside via CLI)


### Option 2

- Write `Dockerfile` and then:

- `docker build -t test-tag .`
- `docker run --name test_container_rhel84 -p 8080:8080 -d test-tag`

<br/>
<br/>
<br/>
<br/>


### Working example
https://developers.redhat.com/blog/2020/03/24/red-hat-universal-base-images-for-docker-users#red_hat_enterprise_linux_and_docker 

Dockerfile:
```
FROM registry.access.redhat.com/ubi8/ubi:8.1

RUN yum --disableplugin=subscription-manager -y module enable php:7.3 \
  && yum --disableplugin=subscription-manager -y install httpd php \
  && yum --disableplugin=subscription-manager clean all

ADD index.php /var/www/html

RUN sed -i 's/Listen 80/Listen 8080/' /etc/httpd/conf/httpd.conf \
  && sed -i 's/listen.acl_users = apache,nginx/listen.acl_users =/' /etc/php-fpm.d/www.conf \
  && mkdir /run/php-fpm \
  && chgrp -R 0 /var/log/httpd /var/run/httpd /run/php-fpm \
  && chmod -R g=u /var/log/httpd /var/run/httpd /run/php-fpm

EXPOSE 8080
USER 1001
CMD php-fpm & httpd -D FOREGROUND
```

index.php
```
<html>
<body>
<?php print "Hello, world!\n" ?>
</body>
</html>
```

`$   docker build -t php-hello .`

`$   docker run --name hello -p 8080:8080 -d php-hello`

To reset container:
```
$ docker stop hello
$ docker rm hello
```
