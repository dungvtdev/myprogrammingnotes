# Linux commands

## apt-get
- apt-get update
    + is restricted to the case where packages are to be replaced by newer versions, but no package needs to be added or removed. A new version of Firefox, for instance, should be installable with apt-get upgrade.
- apt-get dist-upgrade
    + will refuse to work when there are additions or removals required by the updated versions. For example, when you have kernel linux-image-3.2.0-10-generic installed and linux-image-3.2.0-11-generic appears, the linux-image-generic package gets updated to depend on the newer version. In order to install the new kernel, you need to run apt-get dist-upgrade.
- apt-get install PACKAGE
- add-apt-repository REPOSITORY
    + add a repository into the /etc/apt/sources.list or /etc/apt/sources.list.d/ with GPG public key.

## System V script, Upstart job
- service
