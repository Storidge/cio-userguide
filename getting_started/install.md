# Install cio

A convenience script installs the community edition of cio software into your development environments quickly and non-interactively. The use of convenience scripts is recommended for dev environments only, as root permissions are required to run them. This script will detect your Linux distribution and version, and install all dependencies and suggestions of the package manager without asking for confirmation.

Storidge's cio software currently supports CentOS 7.5, 7.6 (3.10 kernel), RHEL 7 (3.10 kernel) and Ubuntu 16.04LTS (4.4 kernel). After verifying you have a supported distribution, run the script below to begin installation.

`curl -fsSL ftp://download.storidge.com/pub/free/demo | sudo bash`

Example:

```
root@c1:~# curl -fsSL ftp://download.storidge.com/pub/free/demo | sudo bash
Started installing release 2148 at Tue Jan 30 12:47:26 PST 2018
Loading cio software for: u16  (4.4.0-104-generic)
...
...
...
latest: Pulling from portainer/portainer
Digest: sha256:232742dcb04faeb109f1086241f290cb89ad4c0576e75197e902ca6e3bf3a9fc
Status: Image is up to date for portainer/portainer:latest
Finished at Tue Jan 30 12:48:04 PST 2018
cio software installation complete. cio requires a minimum of 3 local drives per node for data redundancy

Please verify local drives are available, then run 'cioctl create' command on primary node to start a new cluster
```

**Install Additional Nodes**

Add more nodes to the cluster to increase capacity and enable high availability for your applications. Repeat the convenience script installation on all nodes that will be members of the cluster.