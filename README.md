# docker-container-log-rotate

My notes

## Usecase

mount log files output by the dokcer container on the host, and rotate the log files using host OS logrotated.

## Step

```
# docker run --name nginx \
  -d \
  --publish 80:80 \
  --volume /opt/docker/nginx:/etc/nginx/conf \
  --volume /var/log/nginx:/var/log/nginx \
  nginx:latest

# touch /etc/logrotate.d/nginx.logrotate
# vi /etc/logrotate.d/nginx.logrotate
# cat /etc/logrotate.d/nginx.logrotate
/var/log/nginx/*log {
    daily
    rotate 30
    missingok
    notifempty
    sharedscripts
    compress
    delaycompress
    postrotate
        docker kill -s USR1 nginx >/dev/null 2>&1
    endscript
}

# logrotate -f /etc/logrotate.conf
```
