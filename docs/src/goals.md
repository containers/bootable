# Image Based Linux with Bootable container images

Over the last decade, OCI containers have become a de facto way to deploy a complete functioning Linux user space as an application.
A large set of practices and tooling has evolved around them.
Bootable containers are a modern opinionated way of deploying, configuring and managing immutable image based Linux systems.

Our goals are:

1. Use standard container practices and tooling, such as the OCI standard, layering, container registries, signing, testing, and GitOps workflows to build Linux systems.

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


