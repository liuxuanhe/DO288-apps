FROM centos:centos7
MAINTAINER Red Hat Training <training@redhat.com>
# Generic labels
LABEL Component="httpd" \
 Name="do288/httpd-parent" \
 Version="1.0" \
 Release="1"
# Labels consumed by OpenShift
LABEL io.k8s.description="A basic Apache HTTP Server image with ONBUILD instructions" \
 io.k8s.display-name="Apache HTTP Server parent image" \
 io.openshift.tags="apache, httpd"
# DocumentRoot for Apache
ENV DOCROOT=/var/www/html \
 LANG=en_US \
 LOG_PATH=/var/log/httpd
# Need this for installing Apache from classroom yum repo
ADD Centos-7.repo /etc/yum.repos.d/training.repo
RUN yum install --noplugins httpd -y && yum clean all --noplugins -y && echo "Hello from the httpd-parent container!" > ${HOME}/index.html
# Allows child images to inject their own content into DocumentRoot
ONBUILD COPY src/ /var/www/html/
RUN sed -i "s/Listen 80/Listen 8080/g" /etc/httpd/conf/httpd.conf && chgrp -R 0 /var/log/httpd /var/run/httpd && chmod -R g=u /var/log/httpd /var/run/httpd && rm -rf /run/httpd && mkdir /run/httpd

USER 1001
EXPOSE 8080

LABEL io.openshift.expose-services="8080:http"
# Launch apache daemon
CMD /usr/sbin/apachectl -DFOREGROUND
