
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
Create a directory where you want to mount the shared directory. For example, we can create a directory called nfs_share in the /mnt directory. And mount the shared directory using the following command:
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

* Log in to Proxmox.
* Select Create CT to create a new container.
* Enter a Hostname, then enter the Password you’d like to use. This password will be used to log in to the root user account. After all the settings have been specified, select Next.
* Select the latest Debian Template, then select Next to proceed.
* Select the Disk Size for this container, then select Next.
* Select the total Cores for the CPU, then select Next.
* Set the total Memory, then select Next.
* Change the Network to use DHCP for IPv4 and IPv6 (unless you want to specify them manually), then select Next until you get to Confirm.
* Optional: Configure DNS section
* Confirm the settings, then select Finish to create the container. But don't select 'Start after created' to auto-start the container.
* Select the LXC Container we just created, then select Options and Edit the Features.
* Enable keyctl, then select OK. You can now start the container!
* After the container starts, log in with the username root and password set in step four. Run the command below to update the system.

```console
apt update && apt upgrade -y
```
## Install Docker & Portainer
* Install Curl
```console
apt install curl -y
```
* Install Docker
```console
curl -sSL https://get.docker.com/ | sh
```
* Install Portainer
```console
docker run --restart always -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
* Check the IP Address of the LXC container
```console
ip addr
```

* After portainer is installed you can access portainer in your browser useing:
```console
http://[CONTAINER_IP]:9000
```

## Install Nextcloud using docker-compose via Portainer

### Option 1 - Linuxserver.io
* Select Stacks, and Add Stack to create a new stack.
* Create the stack and set the name as 'nextcloud'.
* Paste the contents below into the web editor.
```console
version: "2.1"
services:
  nextcloud:
    image: lscr.io/linuxserver/nextcloud:latest
    container_name: nextcloud
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /path/to/appdata:/config
      - /path/to/data:/data
    ports:
      - 443:443
    restart: unless-stopped
```
Parameters:
-p 443 	WebUI
-e PUID=1000 	for UserID - see below for explanation
-e PGID=1000 	for GroupID - see below for explanation
-e TZ=Etc/UTC 	specify a timezone to use, see this list.
-v /config 	Nextcloud configs.
-v /data 	Your personal data.


Notes: User / Group Identifiers
In this instance PUID=1000 and PGID=1000, to find yours use id your_user as below:
```console
id your_user
```
Example output:
```console
uid=1000(your_user) gid=1000(your_user) groups=1000(your_user)
```

Docker cli
```console
docker run -d \
  --name=nextcloud \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Etc/UTC \
  -p 443:443 \
  -v /path/to/appdata:/config \
  -v /path/to/data:/data \
  --restart unless-stopped \
  lscr.io/linuxserver/nextcloud:latest
```

### Option 2 - Mariadb and Nextcloud
* Select Stacks, and Add Stack to create a new stack.
* Create the stack and set the name as 'nextcloud'.
* Paste the contents below into the web editor.
```console
version: '2'

services:
  db:
    image: mariadb:10.5
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - /var/lib/docker/volumes/Nextcloud_Database:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_PASSWORD=password
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud

  app:
    image: nextcloud
    restart: always
    ports:
      - 8080:80
    links:
      - db
    volumes:
      - /var/lib/docker/volumes/Nextcloud_Application:/var/www/html
    environment:
      - MYSQL_PASSWORD=password
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db
```


## Install OnlyOffice

## (Optional) Install Watchtower (docker)

* Select Stacks, and Add Stack to create a new stack.
* Create the stack and set the name as 'w
* atchtower'.
* Paste the contents below into the web editor.
```console

```

## (Optional Scenario) Cloudflare DDNS

## (Optional Scenario) DNS-01 Certificates - Non or VPN External Exposure

## (Optional Scenario) External Exposure
