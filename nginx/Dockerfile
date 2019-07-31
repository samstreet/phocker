#
# docker build -t spafftank/phocker-nginx -f ./nginx/Dockerfile ./nginx
#

FROM nginx:latest

WORKDIR /etc/nginx

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf ./nginx.conf
COPY conf.d/ ./conf.d/
COPY sites-enabled/ ./sites-enabled/

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
