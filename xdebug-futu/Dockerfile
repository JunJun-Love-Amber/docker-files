FROM fangdany/xdebug:7.0

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

# 安装 protobuf（仅用于富途）
RUN git clone https://github.com/allegro/php-protobuf; \
	cd php-protobuf; \
	phpize ; \
	./configure; \
	make; \ 
	make install; \
	docker-php-ext-enable protobuf

# 安装 tideways
RUN echo 'deb http://s3-eu-west-1.amazonaws.com/tideways/packages debian main' > /etc/apt/sources.list.d/tideways.list; \
	apt update; \
	apt install -y gnupg ;\
	wget -qO - https://s3-eu-west-1.amazonaws.com/tideways/packages/EEB5E8F4.gpg | sudo apt-key add -; \
	apt-get update; \
	apt-get install tideways-php; \
	docker-php-ext-enable tideways

# 安装 mongodb
RUN pecl install mongodb; docker-php-ext-enable mongodb; rm -rf /tmp/pear



