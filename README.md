
> [!CAUTION]
> This repository is no longer maintained as of May 18th, 2025.

# Portainer + Traefik w/ Let's Encrypt

This repository will help you set up Portainer served by Traefik over HTTPS (Let's Encrypt)

**Make sure that Docker Engine & Docker compose are installed in your server**

- [Docker Engine](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

- [Docker Compose](https://docs.docker.com/compose/install/)

## Setup

Simply `git clone` the repository into your server's directory.

`git clone https://github.com/officialmofabs/portainer-traefik-letsencrypt.git`

  Apply permissions to be able to edit the `.env`.

Then fill the `.env` file with your credentials and your domain:

```
#GENERAL INFO
ACME_EMAIL=email@domain.com
PORTAINER_TRAEFIK_URL=whatever.mydomain.com
```

* `ACME_EMAIL` is the email address which will be used when registering the certificate at Let's Encrypt.
* `ODOO_TRAEFIK_URL` is the hostname which Traefik will listen to route requests.

## Run

```bash
cd portainer-traefik-letsencrypt
docker-compose up -d
```

**NOTE:** Traefik will automatically renew the certificate every 3 months.

## Exposing Docker sockets via portainer-agent (Optional)

Add the portainer/agent service to your Docker Compose stack on the host you want to manage, reup the stack with `docker-compose up -d` and then in the manager host, just connect to the WAN IP which has port 9001 forwarded in Portainer. 


```
portainer_agent:
    image: portainer/agent
    container_name: portainer_agent
    ports:
      - "9001:9001"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro  # Mount with read-only access
      - /var/lib/docker/volumes:/var/lib/docker/volumes:ro  # Mount with read-only access
    restart: always

```
