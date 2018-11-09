FROM php:7.0-fpm

# 安装常用工具
RUN apt update; \
	apt install vim net-tools iputils-ping htop iotop curl git zip procps wget sudo lsof telnet -y; \
	rm -rf /var/lib/apt/lists/*
# 启用 ll 
RUN php -r '$bashrc="/root/.bashrc";file_put_contents($bashrc,str_replace("# alias ll","  alias ll",file_get_contents($bashrc)));'

# 修改 php-fpm 配置
RUN sed -i 's/\;pid = run\/php-fpm.pid/  pid = run\/php-fpm.pid/g' /usr/local/etc/php-fpm.conf; \
	sed -i 's/\;error_log = log\/php-fpm.log/  error_log = log\/php-fpm.log/g' /usr/local/etc/php-fpm.conf; \
	sed -i 's/\; process.max = 128/  process.max = 128/g' /usr/local/etc/php-fpm.conf

RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# 安装 xdebug
RUN pecl install xdebug; \
	docker-php-ext-enable xdebug; \
	rm -rf /tmp/pear

# 安装依赖与扩展
RUN	requirements="libmcrypt-dev libcurl4-gnutls-dev libpng-dev zlib1g-dev"; \ 	
	apt-get update; apt-get install -y $requirements; \
	docker-php-ext-install -j$(nproc) \
		gd bcmath pdo_mysql mcrypt  pcntl sockets zip; \
	rm -rf /var/lib/apt/lists/*

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    composer self-update && \
    composer config -g repo.packagist composer https://packagist.phpcomposer.com

# 安装 igbinary redis
RUN pecl install igbinary redis; \
	yes | pecl install redis; \
	docker-php-ext-enable igbinary redis; \
	rm -rf /tmp/pear

# 安装 imap bz2
RUN apt-get update; \
	apt-get install -y libc-client-dev libkrb5-dev libbz2-dev; \
	rm -r /var/lib/apt/lists/*; \
	docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap bz2;\
	rm -rf /tmp/pear

# 安装 tideways
RUN echo 'deb http://s3-eu-west-1.amazonaws.com/tideways/packages debian main' > /etc/apt/sources.list.d/tideways.list; \
	apt update; \
	apt install -y gnupg ;\
	wget -qO - https://s3-eu-west-1.amazonaws.com/tideways/packages/EEB5E8F4.gpg | sudo apt-key add -; \
	apt-get update; \
	apt-get install tideways-php; \
	docker-php-ext-enable tideways

# 安装 tideways_xhprof
RUN cd /usr/local/src; \
	git clone https://github.com/tideways/php-xhprof-extension.git; \
	cd php-xhprof-extension/ ;\
	phpize; \
	./configure; \
	make; \ 
	make install; \
	docker-php-ext-enable tideways_xhprof

# 安装 mongodb
RUN pecl install mongodb; docker-php-ext-enable mongodb; rm -rf /tmp/pear

# 安装 nginx
RUN apt update; apt install nginx -y; rm -rf /var/lib/apt/lists/*

WORKDIR /codes

# php-fpm xdebug
EXPOSE 9000 80 443 22

