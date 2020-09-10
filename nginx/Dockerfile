# Create image based on the official Nginx
FROM nginx:latest
SHELL ["/bin/bash", "-c"]
# Install software
RUN apt-get update && \
    apt-get install -y openssl && \
    apt-get install -y libnss3-tools
# Create certificate
RUN openssl req \
    -newkey rsa:2048 \
    -x509 \
    -nodes \
    -keyout /etc/ssl/private/nginx-selfsigned.key \
    -new \
    -out /etc/ssl/certs/nginx-selfsigned.crt \
    -subj /CN=localhost \
    -reqexts SAN \
    -extensions SAN \
    -config <(cat /etc/ssl/openssl.cnf \
        <(printf '[SAN]\nsubjectAltName=DNS:localhost')) \
    -sha256 \
    -days 3650
# Install certificate
RUN mkdir -p $HOME/.pki/nssdb && \
    certutil -d sql:$HOME/.pki/nssdb -A -t "CT,c,c" -n "localhost" -i /etc/ssl/certs/nginx-selfsigned.crt
# Add file configuration
ADD conf/nginx.conf /etc/nginx/conf.d/default.conf