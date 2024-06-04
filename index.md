---
title: Goals
layout: home
---

## Image Based Linux with Bootable Container Images

Over the last decade, [OCI containers](https://specs.opencontainers.org/image-spec/) have become a de facto way to deploy a complete functioning Linux user space as an application.
A large set of practices and tooling have evolved around them.
Bootable containers are a modern opinionated way of deploying, configuring and managing immutable image based Linux systems.

Our goals are:

1. Use standard container practices and tooling, such as the [OCI standard](https://specs.opencontainers.org/image-spec/), layering, container registries, [signing](https://docs.sigstore.dev/signing/signing_with_containers/), testing, and GitOps workflows to build Linux systems.

1. Container images describe the operating system behavior as a prebuilt predefined unit, rather than defined during deployment out of fine grained packages.
There is a strong bias toward having the full system definition committed to version control, including a list of components, application files and system configuration.  This bias helps implement the concept of a more composable operating system.

1. The system updates atomically.
It is robust to power outages or software failures during updates.
The system either uses the contents of the old system, or the new image; Never some combination of both.

1. The operating system should self-update when new images are available, as system source of truth should default to a remote registry.
Updates can be delayed or scheduled.
This default behavior can be adapted or controlled by a larger management system.

1. If an update does not function correctly it is possible to roll back to the container image previously functioning before the update.

1. State (including per-machine configuration) is preserved across updates.
State is written to specific writable directories on the system, by default these are /etc and /var.

1. It should always be possible to factory reset back to the known built behavior of the system, by discarding all state (and per machine configuration) not present in the container image.

1. It should be supported to create a cryptographic trust chain that runs from the hardware, through the boot loader, through the operating system all the way to the applications ensures that only the expected code is run, and the contents of the operating system and applications have not been changed unexpectedly.
If something has been changed, or changes at runtime unexpectedly, the system can alert or stop.
The builder of the images can sign the images with keys that are under their own control, or of course build images and deploy systems without a trust chain.  But, it also continues to be supported by system builders to create "unlocked" systems where the end user can make local unsigned changes.

1. Updates are the quintessential operation for deploying bootable containers.
It is tempting to include any and all system deployment information in a bootable container artifacts, definitions or images.
Only configuration and deployment choices that can be successfully deployed via a new container image update to an existing system, or via a factory reset, are those that belong in the bootable container definitions or images.
A canonical example is disk partition layout: this attribute of a deployment cannot be changed via an updated container image, and thus is the responsibility of an installer or infrastructure providing the system, rather than bootable containers.
