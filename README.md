# Nextcloud on Portainer
How to install Nextcloud using MariaDB and onlyoffice in a home scenario on proxmox.

## Goals
This guide will help you accomplish the following goals:
* Connect Proxmox to NFS Share
* Setup LXC Debian Container
* Install Portainer
* Install MariaDB using docker-compose via Portainer
* Install Nextcloud using docker-compose via Portainer
* Install OnlyOffice
* (Optional Scenario) Cloudflare DDNS
* (Optional Scenario) DNS-01 Certificates - Non or VPN External Exposure
* (Optional Scenario) External Exposure

## Assumptions
This guide makes the following assumptions:
* Installed and updated Proxmox installation
* NFS Share somehere in your network (preferrably NAS)

## Connect to NFS Share

### Linux NFS Connect

### Proxmox NFS Connect

## Setup LXC Debian Container

## Install Docker & Portainer

```console
apt update && apt upgrade -y
apt install curl -y
curl -sSL https://get.docker.com/ | sh
docker run --restart always -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```

## Install MariaDB using docker-compose via Portainer

## Install Nextcloud using docker-compose via Portainer

## Install OnlyOffice

## (Optional Scenario) Cloudflare DDNS

## (Optional Scenario) DNS-01 Certificates - Non or VPN External Exposure

## (Optional Scenario) External Exposure
