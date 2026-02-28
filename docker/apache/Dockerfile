FROM httpd:2.4-alpine

# Install dependencies
RUN apk update && apk add --no-cache curl bash tzdata

# 1. Copy Apache Configuration (requires access to docker folder)
COPY docker/apache/conf/httpd.conf /usr/local/apache2/conf/httpd.conf
RUN mkdir -p /usr/local/apache2/conf/sites-available
COPY docker/apache/conf/opencart.conf /usr/local/apache2/conf/sites-available/opencart.conf

# 2. Copy Application Source Code into the image
# This populates /var/www so Docker can initialize the empty volume on first run
COPY . /var/www

# 3. Security & Cleanup
# Remove the docker config folder from the final web directory
# Remove install folder if you want to lock down production (optional)
RUN rm -rf /var/www/docker

EXPOSE 80
