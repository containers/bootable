---
title: How does it work?
layout: page
---

## How do bootable container images work?

Any OCI container build pipeline can be used to (re)build the container images from that system definition in version control build pipelines, tested, and pushed to a typical OCI container registry.
In order to be bootable, these container images include their own Linux kernel and a system manager, usually [systemd](https://systemd.io/).
The resulting image is an OCI image in every way.
In fact it can be started as a typical container in [podman](http://podman.io/) or [Docker](https://www.docker.com/), without booting it, but of course without running the included kernel.

The [bootc tool](https://github.com/containers/bootc) applies a container image as an update on an already running Linux system.
The contents are written to the existing filesystem, switching out the /usr and /boot directories of the Linux system.
One then reboots into the new behavior of the system.
The new kernel in the bootable container image must of course support the filesystem, and storage devices on the system.

Using the boot loader it is possible to switch back to the previous operating system image, thus enabling rollback to the previous behavior of the system.
Using this methodology, it’s expected to be able to start with a stock Linux system, and completely change the behavior of that system using the new bootable container image, and then iterate as with further images.

By default, such systems automatically update themselves when a new version of their bootable container image is tagged in the container registry.

Per system state and data are set up per system in /etc and /var per system, and is usually not affected by updates to the bootable container image.
This includes things like passwords, SSH keys, home directories.
These things can arrive on the system in the typical way, such as local configuration, cloud-init, or via management tools like Ansible.

It is recommended to have the container image content be read only at runtime for both the operating system and application containers.
It will be possible and supported to use a local overlay file system as well as a commit process to enable debugging.

It is recommended that general purpose bootable containers ship with at least a minimal installation tool that can execute from the container itself and write to a block device, similar to a “self-extracting zip file”.
However it must also be a first class operation to generate disk images (ISO, qcow2, etc) from bootable containers for general purpose operating systems.
Disk images should include just content derived from the input image, plus “bootstrap secrets” or similar where necessary.
It should be a rare operation to need to consider the initial disk image contents to understand the state of the system.

Stock disk images should be provided by Linux distributions for exactly this use case.
Users are encouraged to then update that system with their own bootable container image.


