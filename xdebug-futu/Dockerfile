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



# php-fpm xdebug http https sshd
EXPOSE 9000 80 443 22 8057
