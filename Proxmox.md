# Proxmox

## Source  links:

- [https://pve.proxmox.com/wiki/Unprivileged_LXC_containers](https://pve.proxmox.com/wiki/Unprivileged_LXC_containers)
- [https://github.com/ahuacate/proxmox-lxc-homelab#100-unprivileged-lxc-containers-and-file-permissions](https://github.com/ahuacate/proxmox-lxc-homelab#100-unprivileged-lxc-containers-and-file-permissions)

## Useful nano commands:
```
Ctrl + o --> save changes
Ctrl + x --> exit nano
Ctrl + k --> delete line
```


## How to setup shared directory on Proxmox host to be used in multiple LXC containers (mount)

1. On Proxmox create new user group (like **homegroup**) in Datacenter
2. Create new user (like **homeuser**)

![Proxmox user](images/Proxmox_user_group.jpg)

3. Create new user on node through command line 
```
groupadd homegroup &&
useradd  -g homegroup -m homeuser
```
- to find out what ids created user has, do `id homeuser`

4. Create new `zfs pool`
- execute for example `zfs create DATA3TB/homeshare/share`
5. Give to the new created user ownership of this folder
```
chgrp -R homegroup /DATA3TB/homeshare/share
chown -R homeuser /DATA3TB/homeshare/share
```
6. Then create new lxc container with ubuntu (for example with `id: 203`).
7. Now we need to edit `203.conf`

    Location of every **conf** file is in `/etc/pve/lxc/*.conf`
    To edit `203.conf` we execute 
```
nano /etc/pve/lxc/203.conf
```
- We need to add following lines 
```
mp0: /DATA3TB/homeshare/share/,mp=/mnt/share
lxc.idmap: u 0 100000 1607
lxc.idmap: g 0 100000 1000
lxc.idmap: u 1607 1607 1
lxc.idmap: g 1000 1000 1
lxc.idmap: u 1608 101608 63928
lxc.idmap: g 1001 101001 64535
```
- First line `mp0...`defines the new share
- other lines assume that the user `homeuser` and his group `homegroup`have id´s of `user = 1607` and `group = 1000`
8. Only once per node (**proxmox server**) following files have also to be edited:
```markdown
/etc/subuid
/etc/subgid
```
- In both files we add lines:
  - in subuid `root:1607:1`
  - in subgid `root:1000:1`

9. To test if the mount and rights has worked, we test it when we login into lcs container and execute `df -h`.

    We must see our share `/mnt/share/`
10. Then we can try to create new file or folder in that location `mkdir /mnt/share/testfolder`

    If we don´t receive any error, then everything is fine.
11. In lcx container install samba for sharing with `apt update && apt install samba -Y`
12. After that we modify samba config file
```
mv /etc/samba/smb.conf /etc/samba/smb.bak
nano /etc/samba/smb.conf
```
13. The configration looks like this 
```Nginx
[global]
workgroup = WORKGROUP

[data]
path = /mnt/share
writeable = yes
browseable = yes
valid user = homeuser
```
14. After every change of `smb.conf` file, we need to restart samba service with `systemctl restart smbd.service`

15. We create new samba user and linux user **with same password**.
```
smbpasswd -a homeuser
groupadd -g 1000 homegroup &&
useradd -u 1607 -g homegroup -m homeuser
passwd homeuser
```


16. Docker on Proxmox with portainer 
```
apt update
 
apt install docker.io
systemctl start docker
systemctl enable docker
 
### install docker-compose ###
 
apt install curl
 
curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 
chmod +x /usr/local/bin/docker-compose
 
docker-compose --version
 
## install portainer ##
 
docker volume create portainer_data
 
docker run -d -p 9100:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /data/configs/dockerconfig/portainer:/data -e TZ=Europe/Berlin  portainer/portainer:latest
```
17. Create cifs (samba share) in ubuntu VM (not LXC!)

[Ubuntu CIFC share in VM](https://gist.github.com/ipbastola/eaa107b4640262a108fcc3ef57d24836)

18. Create nfs (synology share) in ubuntu VM.

[Ubuntu NFS share in VM](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-18-04#step-5-%E2%80%94-creating-mount-points-and-mounting-directories-on-the-client)

19. If the current user in Ubuntu has only `$` in shell, then follwoing command has to be executed to repair it: 

`chsh -s /bin/bash john`

