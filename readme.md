## Guide

Switch from Docker (not works podman! ) TO VAGRANT VM (finally it works!)

# Vagrant Box VM on top of VirtualBox with: Rhel8
Scaffolding to make VirtualBox/Vagrant VM with:
- RedHat 8 (Rhel 8),
- vagrant user/group configuration (password for vagrant user: `vagrant`)
- cockpit - browser web console via http://localhost:9090  

<br/>

## Full Environment scaffolding
#### Run provisioner script, passing some env variables:

1) On Linux

`RHEL_USERNAME='username' RHEL_PASSWORD='password' vagrant up --provision`

or

`RHEL_USERNAME='username' RHEL_PASSWORD='password' vagrant reload --provision` (Imp: use single quotes for pswd)

<br/>

<br/>

<br/>

<br/>

<br/>

Rhel username & password are your subscription-manager credential to rhel 8 site

https://access.redhat.com/management

#### If already provisioned, simply:
`vagrant up`

<br/>
<br/>

## To login into Cockpit browser web console:
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



# OLD_ Docker (podman not works)

```
docker-compose up --build
```

in new tab:

`$  docker ps` ---> to see <container-ID>

``$  docker exec -it <container-ID> bash``

you are inside new clean redhat container with `root` user

`cat /etc/os-release`  --> to verify current OS distrib & vers


### Working example with docker (NO docker-compose)
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

`$   docker build --rm -t test .`

`$   docker run -it --name test_name -p 8080:8080 -d test`

To reset container:
```
$ docker stop hello
$ docker rm hello
```
