FROM redis:4.0

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN chmod -R 777 /var/log
COPY redis.conf /etc/redis/redis.conf
CMD [ "redis-server", "/etc/redis/redis.conf" ]