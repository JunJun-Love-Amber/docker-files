FROM php:7.0-fpm

# 安装常用工具
RUN apt update; \
	apt install vim net-tools iputils-ping htop iotop curl git zip procps wget sudo lsof -y; \
	rm -rf /var/lib/apt/lists/*
# 启用 ll 
RUN php -r '$bashrc="/root/.bashrc";file_put_contents($bashrc,str_replace("# alias ll","  alias ll",file_get_contents($bashrc)));'
# 修改 php-fpm 配置
RUN sed -i 's/\;pid = run\/php-fpm.pid/pid = run\/php-fpm.pid/g' /usr/local/etc/php-fpm.conf

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

WORKDIR /codes

# php-fpm
EXPOSE 9000
# xdebug 
EXPOSE 9001

# php-fpm# php-fpm# php-fpm