---
title: Projects involved
layout: page
---

## What are the projects involved or related?

The bootc tooling and project are the nexus of a lot of the functionality that enable containers to be used to deploy Linux systems:

 * [bootc documentation](https://containers.github.io/bootc/)
 * [bootc on Github](https://github.com/containers/bootc)

Obviously all the usual container tooling should be used to build images, such as [Podman](http://podman.io/), [Docker](https://www.docker.com/), [Quay](https://quay.io/), [Docker Hub](https://hub.docker.com/), and so on.
The Image Builder project can be used to convert bootable container images to disk images when necessary:

 * [bootc-image-builder on Github](https://github.com/osbuild/bootc-image-builder)

Packages and software should use the various standards that together work toward making an immutable and hermetic. Each of these work toward the same principles that make software behave better bootable containers and containers in general.

 * [UAPI: Linux Userspace API Group](https://uapi-group.org/)
 * [Config file specification](https://uapi-group.org/specifications/specs/configuration_files_specification/), contribute to [Particle OS](https://0pointer.net/blog/fitting-everything-together.html)
 * Use [systemd-sysusers](https://www.freedesktop.org/software/systemd/man/latest/systemd-sysusers.html) or [DynamicUser=yes](https://0pointer.net/blog/dynamic-users-with-systemd.html)
 * RPM packaging: Move from `%post` to systemd units

The composefs, overlayfs, fs-verity and UKI provide the basic technology to pursue the trust chain work.

 * [composefs on Github](https://github.com/containers/composefs)
 * [overlayfs documentation](https://www.kernel.org/doc/Documentation/filesystems/overlayfs.txt)
 * [fs-verity documentation](https://www.kernel.org/doc/html/next/filesystems/fsverity.html)
 * [UKI specification](https://github.com/uapi-group/specifications/blob/main/specs/unified_kernel_image.md)

Podman Desktop can be used to get started with bootable container images:

 * [Podman Desktop bootc extension](https://github.com/containers/podman-desktop-extension-bootc)

Linux workstation distributions like Bluefin and more broadly the Universal Blue project are pursuing this today.

 * [Project Bluefin](https://projectbluefin.io/)
 * [Universal Blue](https://universal-blue.org/)

There are proposals in Fedora to implement bootable container images as an image mode for the operating system.

 * [Fedora bootc images](https://docs.fedoraproject.org/en-US/bootc)
 * [Fedora change proposal](https://fedoraproject.org/wiki/Changes/OstreeNativeContainerStable)
 * [Fedora change Sysusers](https://fedoraproject.org/wiki/Changes/SystemdSysusers)
 * [Fedora change zstd:chunked](https://fedoraproject.org/wiki/Changes/zstd:chunked)
