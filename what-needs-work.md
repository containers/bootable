---
title: What needs work?
layout: page
---

## What needs work?

Broadly there are several areas where we haven’t yet reached our [goals](mission.md), and where you can help:

 * At present, bootable container images must be built from a specific base image despite it being a goal to use standard base images.

 * At present, we can only update a bootc enabled system with a bootable container image. It is not yet possible to use “bootc update” on a stock Linux system.

 * When using “bootc install” to update a non-bootc Linux system, it is not possible to roll back to that previous behavior.

 * The cryptographic trust chain is possible based on composefs, overlayfs, fsverity and UKI use to mount both application containers and the operating system bootable container images. However a working complete trust chain from hardware through to the app containers is not yet implemented.

 * When rebooting these image based Linux systems, all transient changes made to the optional overlay are lost.
This would be confusing to a developer or someone trying to adopt these images.
The behavior is different from the behavior of containers, where you can make changes to a running container, stop and  start that container without losing those local changes.

 * Currently the tooling and the base images are limited to using RPM components in the container images. (See: [https://github.com/coreos/bootupd/issues/468])
