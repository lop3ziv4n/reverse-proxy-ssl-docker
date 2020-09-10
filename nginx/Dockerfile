# Create image based on the official Nginx
FROM nginx
SHELL ["/bin/bash", "-c"]
# Install libreries
RUN apt-get update && \
    apt-get install -y openssl && \
    apt-get install -y libnss3-tools
# Add files configuration
ADD conf/open-ssl.conf /etc/ssl/open-ssl.conf
ADD conf/nginx.conf /etc/nginx/conf.d/default.conf
# Create self-signed certificate
RUN openssl req \
    -newkey rsa:2048 \
    -x509 \
    -nodes \
    -keyout /etc/ssl/private/nginx-selfsigned.key \
    -new \
    -out /etc/ssl/certs/nginx-selfsigned.crt \
    -extensions 'v3_req' \
    -config /etc/ssl/open-ssl.conf \
    -sha256 \
    -days 3650
# Trust self-signed server certificate
RUN mkdir -p $HOME/.pki/nssdb && \
    certutil -d sql:$HOME/.pki/nssdb -A -t "CT,c,c" -n "localhost" -i /etc/ssl/certs/nginx-selfsigned.crt