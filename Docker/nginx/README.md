# Nginx Docker

[![Image Size](https://img.shields.io/docker/image-size/funnyzak/nginx/latest)](https://hub.docker.com/r/funnyzak/nginx/tags)
[![Docker Pulls](https://img.shields.io/docker/pulls/funnyzak/nginx)](https://hub.docker.com/r/funnyzak/nginx)
[![Docker Version](https://img.shields.io/docker/v/funnyzak/nginx/latest)](https://hub.docker.com/r/funnyzak/nginx/tags)

A nginx docker image with secure configurations and some useful modules, such as `ngx_http_geoip_module`, `ngx_http_image_filter_module`, `ngx_http_perl_module`, `ngx_http_xslt_filter_module`, `ngx_mail_module`, `ngx_stream_geoip_module`, `ngx_stream_module`, `ngx-fancyindex`, `headers-more-nginx-module`, etc.

## Installed Modules

- [ngx_http_geoip_module.so](https://nginx.org/en/docs/http/ngx_http_geoip_module.html)
- [ngx_http_image_filter_module.so](https://nginx.org/en/docs/http/ngx_http_image_filter_module.html)
- ngx_http_perl_module.so
- ngx_http_xslt_filter_module.so
- ngx_mail_module.so
- ngx_stream_geoip_module.so
- ngx_stream_module.so
- [ngx-fancyindex](https://github.com/aperezdc/ngx-fancyindex)
- [headers-more-nginx-module](https://github.com/openresty/headers-more-nginx-module)
- ...

More modules can be found in [Dockerfile](https://github.com/funnyzak/nginx-docker/blob/main/Dockerfile).

## Usage

### docker

First, create a `nginx.conf` file in your project directory, and then run the following command:

```bash
docker run -d -t -i --name nginx --restar on-failure \
  -v /path/to/conf.d:/etc/nginx/conf.d \
  -p 1688:80 \
  funnyzak/nginx
```

### docker-compose

First, create a `docker-compose.yml` file in your project directory:

```yaml
version: “3.3”
services:
  nginx:
    image: funnyzak/nginx
    container_name: nginx
    environment:
      - TZ=Asia/Shanghai
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d
    restart: on-failure
    ports:
      - “1688:80” 
```

Now, running `docker-compose up -d` will run the application for you.

### Docker Build

```bash
docker build \
--build-arg VCS_REF=`git rev-parse --short HEAD` \
--build-arg BUILD_DATE=`date -u +"%Y-%m-%dT%H:%M:%SZ"` \
--build-arg VERSION="1.23.3"
-t funnyzak/nginx:1.23.7 .
```

## Conf Configuration

### Nginx.conf

```conf
load_module /usr/lib64/nginx/modules/ngx_http_image_filter_module.so;
load_module /usr/lib64/nginx/modules/ngx_stream_module.so;
load_module /usr/lib64/nginx/modules/ngx_http_geoip_module.so;
load_module /usr/lib64/nginx/modules/ngx_http_xslt_filter_module.so;
load_module /usr/lib64/nginx/modules/ngx_stream_geoip_module.so;
# load_module /usr/lib64/nginx/modules/ngx_http_perl_module.so;
# load_module /usr/lib64/nginx/modules/ngx_mail_module.so;

user  nginx;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  ‘$remote_addr - $remote_user [$time_local] “$request” ‘
    #                  ‘$status $body_bytes_sent “$http_referer” ‘
    #                  ‘”$http_user_agent” “$http_x_forwarded_for”’;

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

## Reference

- [MaxMind (GeoIp Data)](https://www.maxmind.com/en/accounts/288367/geoip/downloads)
- [Nginx Book](https://ericrap.notion.site/Nginx-1c32ea493c134c36977d8fbd14226079)
- [Nginx Help](https://docs.nginx.com/)