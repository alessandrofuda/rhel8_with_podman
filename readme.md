## Vagrant Box VM on top of VirtualBox with: RHEL8 + Podman
Architecture scaffolding to make VirtualBox/Vagrant VM with on-board:
- RedHat 8 (Rhel 8),
- Podman containerisation system
- Main Nginx Container to Reverse Proxy (expose ports 443/80)
- Secondaries containers for each application
- vagrant user/group configuration (password for vagrant user: `vagrant`)
- cockpit - browser web console via http://localhost:9090  

<br/>

## Full Containers based Environment scaffolding
<br/>

#### 1) Run provisioner script, passing some env variables:

On Linux:

`RHEL_USERNAME='username' RHEL_PASSWORD='password' vagrant up --provision`

or

`RHEL_USERNAME='username' RHEL_PASSWORD='password' vagrant reload --provision` (use single quotes for pswd)

`username` & `password` are the subscription-manager credentials from RHEL 8 website: https://access.redhat.com/management.

<br/>
<br/>

#### 2) If already provisioned, simply run:

`vagrant up`

<br/>
<br/>

#### 3) To login into Cockpit browser web console:
http://localhost:9090

User: `vagrant`

Password: `vagrant`

(from here, it can switch to root user - admin privileges)

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

---



### OLD: Podman NOT WORKS inside RedHat Docker container (inside host: Ubuntu Linux), need full VM provisioning.

`$  docker-compose up --build`

`$  docker ps` ---> to see <container-ID>

`$  docker exec -it <container-ID> bash`

you are inside new clean redhat container like `root` user

`cat /etc/os-release`  --> to verify current OS distrib & vers


#### Working example with docker (without Podman)

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

`$   docker build --rm -t test .`

`$   docker run -it --name test_name -p 8080:8080 -d test`

To reset container:
```
$ docker stop hello
$ docker rm hello
```
