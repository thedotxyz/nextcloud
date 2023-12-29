
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

```console
sudo mkdir /mnt/nfs_share
sudo mount -t nfs x.x.x.x:/nfs_share /mnt/nfs_share 
```

Verify that the share is moutend succesfullie using the mount command:

```console
mount | grep nfs_share 
```

You should see something like this:

```console
192.168.1.100:/nfs_share on /mnt/nfs_share type nfs (rw,relatime,vers=3,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,mountaddr=192.168.1.100,mountvers=3,mountport=20048,mountproto=tcp,local_lock=none,addr=192.168.1.100)
```

### Proxmox NFS Connect
Follow the following instructions to configure a connection from proxmox to an NFS share (Checked Proxmox v8).

* Login to Proxmox
* Select 'Datacenter'
* Select 'Storage'
* Click 'Add'
* Select 'NFS'
* ID: Type in an ID number (own choice)
* Server: Type in the Server IP or domain name
* Export: Select the appropriate share
* Container: Select types of content. FOr instance: Container and/or Snippets

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
