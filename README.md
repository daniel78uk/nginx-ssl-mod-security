# Very secured Nginx with mod_security and SSL Docker image

## What is it

This Dockerfile gives you a ready to use secured production nginx server, with perfectly configured SSL. You should get a A+ Rating at the Qualys SSL Test.

## Environment variables and defaults

* __DH\_SIZE__
 * default: 2048 (which takes a long time to create), for demo or unsecure applications you can use smaller values like 512

## Running nginx-mod_security Container

This Dockerfile is not really made for direct usage. It should be used as base-image for your nginx project. But you can run it anyways.

You should overwrite the _/etc/nginx/external/_ with a folder, containing your nginx __\*.conf__ files, certs and a __dh.pem__.
_If you forget the dh.pem file, it will be created at the first start - but this can/will take a long time!_

    docker run -d \
    -p 80:80 -p 443:443 \
    -e 'DH_SIZE=512' \
    -v $EXT_DIR:/etc/nginx/external/ \
    nginx-mod_security

## Based on

This Dockerfile bases on the centos Official Image.

## Cheat Sheet

### Creating the dh4096.pem with openssl

To create a Diffie-Hellman cert, you can use the following command

    openssl dhparam -out dh4096.pem 4096

### Creating a high secure SSL CSR with openssl

This cert might be incompatible with Windows 2000, XP and older IE Versions

    openssl req -nodes -new -newkey rsa:4096 -out csr.pem -sha256

### Creating a self-signed ssl cert

Please note, that the Common Name (CN) is important and should be the FQDN to the secured server:

    openssl req -x509 -newkey rsa:4086 \
    -keyout key.pem -out cert.pem \
    -days 3650 -nodes -sha256

## Credits

This image was insiper by the work done on this DockerImage https://github.com/MarvAmBass/docker-nginx-ssl-secure
