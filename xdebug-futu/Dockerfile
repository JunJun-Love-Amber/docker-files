FROM fangdany/xdebug:7.0

# 安装 protobuf（仅用于富途）
RUN git clone https://github.com/allegro/php-protobuf; \
	cd php-protobuf; \
	phpize ; \
	./configure; \
	make; \ 
	make install; \
	docker-php-ext-enable protobuf

# futu.ini
RUN { \
	echo '[futu]'; \
	echo 'svrtype="release"'; \
	echo 'site_location="ml"'; \
	echo 'site_idc="local"'; \
}  | tee /usr/local/etc/php/conf.d/futu.ini

# 修正 xdebug 配置
RUN sed -i 's/xdebug.remote_host=127.0.0.1/xdebug.remote_host=172.18.10.109/g'  /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# php-fpm xdebug http https sshd
EXPOSE 9000 80 443 22 8057
