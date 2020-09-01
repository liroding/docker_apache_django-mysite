FROM ubuntu:16.04
MAINTAINER "Liro <dingyinglai@126.com>"

RUN apt-get update \
    && apt-get install -y -q python3-pip \
    && apt-get install -y vim \
    && apt-get install -y --no-install-recommends git \
    && apt-get install curl \
    && apt-get install -y apache2 \ 
    && apt-get install libapache2-mod-wsgi-py3 \
    && apt-get clean \ 
    && rm -rf /var/lib/apt/list/*


ADD Django-2.0.13.tar.gz /opt/

WORKDIR /opt/
RUN mv Django-2.0.13 sourcecode

WORKDIR /opt/sourcecode
RUN python3 setup.py install


#----------------------------------------#
#replace some file about apache etc file 
#and some file of mysite related file
#----------------------------------------#
WORKDIR /opt
RUN django-admin.py startproject mysite

WORKDIR /opt/mysite

RUN mkdir myscript
COPY build_needfile/runwebserver.sh  /opt/mysite/myscript   
COPY build_needfile/migrate.sh  /opt/mysite
COPY build_needfile/mysql_start.sh  /opt/mysite/myscript
COPY build_needfile/mysql_install.sh  /opt/mysite/myscript
COPY build_needfile/fixmysql.sh  /opt/mysite/myscript

COPY build_needfile/mysite/settings.py  /opt/mysite/mysite/settings.py 
COPY build_needfile/mysite/wsgi.py  /opt/mysite/mysite/wsgi.py 
COPY build_needfile/apache2/apache2.conf  /etc/apache2/apache2.conf  
COPY build_needfile/apache2/mods-available/wsgi.conf  /etc/apache2/mods-available/wsgi.conf 
COPY build_needfile/apache2/ports.conf  /etc/apache2/ports.conf 
COPY build_needfile/apache2/sites-available/mysite.conf  /etc/apache2/sites-available/mysite.conf 
WORKDIR /etc/apache2/sites-enabled
RUN ln -s ../sites-available/mysite.conf ./mysite.conf
#----------------------------------------#
EXPOSE 8080

#----------------------------------------#
#add/replace mysql related file 
#----------------------------------------#
RUN pip3 install pymysql
COPY build_needfile/mysite/__init__.py  /opt/mysite/mysite/__init__.py 
#RUN DEBIAN_FRONTEND=noninteractive apt install -y  mysql-server-5.7 
#RUN apt-get install -y mysql-server
#RUN apt-get install -y mysql-client

WORKDIR /opt/mysite
CMD ["/opt/mysite/myscript/runwebserver.sh"]
