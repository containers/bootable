# What are the projects involved or related?

The bootc tooling and project are the nexus of a lot of the functionality that enable containers to be used to deploy Linux systems:

 * [bootc documentation](https://containers.github.io/bootc/)
 * [bootc on Github](https://github.com/containers/bootc)

Obviously all the usual container tooling should be used to build images, such as [Podman](http://podman.io/), [Docker](https://www.docker.com/), [Quay](https://quay.io/), [Docker Hub](https://hub.docker.com/), and so on.
The Image Builder project can be used to convert bootable container images to disk images when necessary:

 * [bootc-image-builder on Github](https://github.com/osbuild/bootc-image-builder)

The composefs, overlayfs, fs-verity and UKI provide the basic technology to pursue the trust chain work.

 * [composefs on Github](https://github.com/containers/composefs)
 * [overlayfs documentation](https://www.kernel.org/doc/Documentation/filesystems/overlayfs.txt)
 * [fs-verity documentation](https://www.kernel.org/doc/html/next/filesystems/fsverity.html)
 * [UKI specification](https://github.com/uapi-group/specifications/blob/main/specs/unified_kernel_image.md)

Podman Desktop can be used to get started with bootable container images:

 * [Podman Desktop bootc extension](https://github.com/containers/podman-desktop-extension-bootc)

Linux distributions like Bluefin and more broadly the Universal Blue project are pursuing this today.

 * [Project Bluefin](https://projectbluefin.io/)
 * [Universal Blue](https://universal-blue.org/)

There are proposals in Fedora to implement bootable container images as an image mode for the operating system.

 * [Fedora change proposal](https://fedoraproject.org/wiki/Changes/OstreeNativeContainerStable)

## What needs work?

Broadly there are several areas where we haven’t yet reached our [goals](mission.md), and where you can help:

 * At present, bootable container images must be built from a specific base image despite it being a goal to use standard base images.

 * At present, we can only update a bootc enabled system with a bootable container image. It is not yet possible to use “bootc update” on a stock Linux system.

 * When using “bootc install” to update a non-bootc Linux system, it is not possible to roll back to that previous behavior.

 * The cryptographic trust chain is possible based on composefs, overlayfs, fsverity and UKI use to mount both application containers and the operating system bootable container images. However a working complete trust chain from hardware through to the app containers is not yet implemented.

 * When rebooting these image based Linux systems, all transient changes made to the optional overlay are lost.
This would be confusing to a developer or someone trying to adopt these images.
The behavior is different from the behavior of containers, where you can make changes to a running container, stop and  start that container without losing those local changes.

 * Currently when we deploy a bootable container image to a stock Linux system, without bootc already present, it is not possible to rollback. We should fix this.

 * Currently the tooling and the base images are limited to using RPM components in the container images. (bootc #468)
