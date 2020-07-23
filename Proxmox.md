# Proxmox

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

4. 
