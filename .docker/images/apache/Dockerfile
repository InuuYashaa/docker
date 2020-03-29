FROM ubuntu:18.04
MAINTAINER Diego Campos

#https://docs.docker.com/engine/faq/#why-is-debian_frontendnoninteractive-discouraged-in-dockerfiles
ENV DEBIAN_FRONTEND noninteractive

ARG phpversion="7.1"

RUN apt update -y
RUN apt install -y wget curl git vim nano zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
RUN apt install -y language-pack-en-base software-properties-common
RUN LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php
RUN LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/apache2
RUN apt update -y
RUN apt install -y apache2 mysql-client
RUN apt install -y php$phpversion php$phpversion-curl php$phpversion-xml \
		php$phpversion-cli php$phpversion-common php$phpversion-json php$phpversion-gd \
		php$phpversion-opcache php$phpversion-mysql php$phpversion-mbstring php$phpversion-mcrypt php$phpversion-zip \
        php$phpversion-intl php$phpversion-xmlrpc php$phpversion-soap

#https://docs.docker.com/config/containers/multi-service_container/
RUN apt install -y supervisor

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf


# Add image configuration and scripts
ADD start-apache2.sh /start-apache2.sh
ADD run.sh /run.sh
RUN chmod 755 /*.sh
ADD supervisord-apache2.conf /etc/supervisor/conf.d/supervisord-apache2.conf


## Activando modulo
RUN a2enmod rewrite

# Configure /app folder with sample app
RUN mkdir -p /app && rm -fr /var/www/html && ln -s /app /var/www/html

##Activando virtual hosts
ADD vhost/* /etc/apache2/sites-available/
RUN a2dissite 000-default.conf
RUN a2ensite diegocampos.com.conf

EXPOSE 80
CMD ["/run.sh"]
