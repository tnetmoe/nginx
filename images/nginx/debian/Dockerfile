ARG NGINX_VERSION
FROM debian:bullseye-slim as debian

FROM nginxinc/nginx-unprivileged:${NGINX_VERSION}-bullseye as nginx

LABEL maintainer="TNET OSS <oss@tnet.moe>"

ARG NGINX_VERSION
ENV IMAGE_VERSION="$NGINX_VERSION"
LABEL version="$NGINX_VERSION"
ARG DISTRO
ENV DISTRO="${DISTRO}"

# copy rm executable file
COPY --from=debian ["/bin/rm", "./rm"]

USER root
# Modify default nginx.
RUN ["rm", "-r", "/etc/nginx/nginx.conf", "/etc/nginx/conf.d", "/usr/share/nginx/html"]
COPY ["/nginx/config/nginx.conf", "/etc/nginx/nginx.conf"]
COPY ["/nginx/config/default.conf", "/etc/nginx/config/default.conf"]
COPY ["/nginx/www/", "/usr/share/nginx/html/"]

STOPSIGNAL SIGQUIT
USER nginx
CMD ["nginx", "-g", "daemon off;"]