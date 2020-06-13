FROM registry.access.redhat.com/ubi7/ubi AS stage1
LABEL maintainer="Ugur Ata"

COPY nginx.repo /etc/yum.repos.d/nginx.repo

RUN  yum -y update --nogpg
RUN  yum -y install nginx --nogpg
RUN  yum clean all && rm -rf /var/cache/yum/

RUN touch /var/run/nginx.pid
RUN chown -R nginx:0 /var/run/nginx.pid /var/log/nginx/ /var/cache/nginx /usr/share/nginx

# non-priviliged account cannot access ports below 1024 so we change it to 8080 instead
RUN sed -i 's/listen\(.*\)80\(.*\);/listen\8080\2;/g' /etc/nginx/conf.d/default.conf

# clearing all previous layers to compress the base image. Maintainer should keep this base image up to date
FROM scratch
COPY --from=stage1 / /

# nginx user comes with the installation or else it should be created manually with
USER nginx

EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]