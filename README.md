Forgejo for internal lab use in (Bootable) Containers and Virtual Machines
==========================================================================


### Container

```bash
$ podman run -d --name forgejo --hostname ${HOSTNAME}-forgejo \
    --net=ncentre-bridge --ip 10.0.21.150 \
    --cap-add=NET_ADMIN --cap-add=NET_RAW --device=/dev/net/tun --cap-add AUDIT_CONTROL \
    ghcr.io/ducttape-lab/forgejo:latest
```

### Virtual Machine (cloud)
Use [`ducttape`](https://github.com/ducttape-infra/ducttape) to `pull`

```sh
$ ducttape pull ghcr.io/ducttape-lab/forgejo-cloud
```
This places the image file in `${HOME}/.local/share/ducttape/base-images/ghcr.io_ducttape-lab_forgejo-cloud.qcow2`


You can run this image with `ducttape` using Lima with the following command:
```sh
$ ducttape run ghcr.io/ducttape-lab/forgejo-cloud -n forgejo-server
```
or use a tool like `virt-install` or your cloud provider.


### Virtual Machine (bootc)
If you want to use a Virtual Machine based on bootable containers, you can either use a bootc host, like from an AlmaLinux 10 bootc installer ISO or use the disk.qcow2 from the [latest release](https://github.com/ducttape-lab/forgejo/releases/latest) in this project.

```sh
$ sudo bootc switch ghcr.io/ducttape-lab/forgejo:latest
```

