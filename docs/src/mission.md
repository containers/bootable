# Image Based Linux with Bootable container images

Over the last decade, OCI containers have become a de facto way to deploy a complete functioning Linux user space as an application.
A large set of practices and tooling has evolved around them.
Bootable containers are a modern opinionated way of deploying, configuring and managing immutable image based Linux systems.

Our goals are:

1. Use standard container practices and tooling, such as the [OCI standard](https://specs.opencontainers.org/image-spec/), layering, container registries, signing, testing, and GitOps workflows to build Linux systems.

1. Container images describe the operating system behavior as a prebuilt predefined unit, rather than defined during deployment out of fine grained packages.
There is a strong bias toward having the full system definition committed to version control, including a list of components, application files and system configuration.

1. The system updates atomically.
It is robust to power outages or software failures during updates.
The system either uses the contents of the old system, or the new image; Never some combination of both.

1. The operating system should self-update when new images are available, as system source of truth should default to a remote registry.
Updates can be delayed or scheduled.
This default behavior can be adapted or controlled by a larger management system.

1. It should always be possible to factory reset back to both the known built behavior of the system.
It is always possible to rollback to a previous behavior if an updated image does not function correctly.

1. A cryptographic trust chain that runs from the hardware, through the boot loader, through the operating system all the way to the apps ensures that only the expected code is run, and the contents of the operating system and applications have not been changed unexpectedly.
If something has been changed, or changes at runtime unexpectedly, the system can alert or stop.
The builder of the images can sign the images with keys that are under their own control, or of course build images and deploy systems without a trust chain.

## How does it work?

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

