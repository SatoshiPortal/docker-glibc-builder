# docker-glibc-builder

A glibc binary package builder in Docker. Produces a glibc binary package that can be imported into a rootfs to run applications dynamically linked against glibc.

## NOTE: libseccomp2 bug in Docker on arm32

If you want to build on an RPi2/3, you first need to upgrade your libseccomp2 library or you'll end up with a date close to epoch time in your container.

Ref.:
- https://bugs.launchpad.net/cloud-images/+bug/1896443
- https://docs.linuxserver.io/faq#libseccomp
- https://github.com/moby/moby/issues/40734

```
wget http://ftp.ca.debian.org/debian/pool/main/libs/libseccomp/libseccomp2_2.5.1-1_armhf.deb
sudo dpkg -i libseccomp2_2.5.1-1_armhf.deb
```

## Usage

Build a glibc package based on version 2.33 with a prefix of `/usr/glibc-compat`:

    docker run --rm --env STDOUT=1 sgerrand/glibc-builder 2.33 /usr/glibc-compat > glibc-bin.tar.gz

You can also keep the container around and copy out the resulting file:

    docker run --name glibc-binary sgerrand/glibc-builder 2.33 /usr/glibc-compat
    docker cp glibc-binary:/glibc-bin-2.33.tar.gz ./
    docker rm glibc-binary
