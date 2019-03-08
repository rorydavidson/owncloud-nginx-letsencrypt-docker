# owncloud-nginx-letsencrypt-docker

This is a simple repo with information on the a `docker-compose.yml` to run [ownCloud](https://owncloud.org/) with an Nginx proxy and LetsEncrypt using Docker, as I was able to find anything that did everything I needed based on the official documentation from ownCloud and kept separate volumes for data.

## Information sources

This is consolidated based on information from the following places and thanks to them:
- [ownCloud server repository](https://github.com/owncloud-docker/server)
- [LetsEncrypt NGINX Proxy companion](https://hub.docker.com/r/jrcs/letsencrypt-nginx-proxy-companion/)

## Get started

Pretty straightforward, follow these steps...

Set up the necessary environment variables at the command line (or equivalent method on the relevant operating system):

```bash
cat << EOF >| .env
OWNCLOUD_VERSION=10.0
OWNCLOUD_DOMAIN=localhost
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin
HTTP_PORT=8080
EOF
```

The webserver is nginx-proxy and it will listen on ports 80 and 443 by default, redirecting traffic to HTTPS for your ownCloud instance. The HTTP_PORT environment variable sets which port ownCloud itself will listen.

Change the hostname variables above and in the `docker-compose.yml` file as necessary specifically the variables in the owncloud service environment block:

```yml
    environment:
      - VIRTUAL_HOST=local.local.info
      - VIRTUAL_PORT=8080
      - LETSENCRYPT_HOST=local.local.info
      - LETSENCRYPT_EMAIL=x@x.x
```

And then run docker compose up to get going.

```bash
docker-compose up -d
```

You should then be able to access it at the domain name you entered and it will redirect to the https URL with a valid certificate.
