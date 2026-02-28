FROM httpd:2.4-alpine

# Install curl (for healthcheck), bash, and tzdata (for TZ environment variable)
RUN apk update && apk add --no-cache curl bash tzdata

COPY docker/apache/conf/httpd.conf /usr/local/apache2/conf/httpd.conf

RUN mkdir -p /usr/local/apache2/conf/sites-available

COPY docker/apache/conf/opencart.conf /usr/local/apache2/conf/sites-available/opencart.conf

EXPOSE 80
