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
$ sudo bootc switch ghcr.io/ducttape-lab/forgejo-bootc:latest
```

#### Install to an exiting root (like cloud)
This allows you to modify the existing system, converting it into the target container image.
```sh
$ podman run --rm --privileged \
    -v /dev:/dev -v /var/lib/containers:/var/lib/containers -v /:/target \
    --pid=host --security-opt label=type:unconfined_t \
    ghcr.io/ducttape-lab/forgejo-bootc:latest \
    bootc install to-existing-root
```

or use [`system-reinstall-bootc`](https://github.com/bootc-dev/bootc/blob/main/docs/src/man/system-reinstall-bootc.8.md)

```sh
$ dnf -y install system-reinstall-bootc
$ system-reinstall-bootc ghcr.io/ducttape-lab/forgejo-bootc:latest
$ reboot
```

> [!NOTE]
> This will create a new SSH key that your client might not like. Use `ssh-keygen -R` to remove the entries.
