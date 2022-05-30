# How to build the xPack GNU AArch64 Embedded GCC binaries

## Introduction

This project also includes the scripts and additional files required to
build and publish the
[xPack GNU AArch64 Embedded GCC](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack) binaries.

It follows the official
[Arm](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain)
distribution, and it is planned to make a new release after each future
Arm release.

Currently the build procedure uses the _Source Invariant_ archive and
the configure options are the same as in the Arm build scripts.

The build scripts use the
[xPack Build Box (XBB)](https://xpack.github.io/xbb/),
a set of elaborate build environments based on a recent GCC (Docker containers
for GNU/Linux and Windows or a custom folder for MacOS).

There are two types of builds:

- **local/native builds**, which use the tools available on the
  host machine; generally the binaries do not run on a different system
  distribution/version; intended mostly for development purposes;
- **distribution builds**, which create the archives distributed as
  binaries; expected to run on most modern systems.

This page documents the distribution builds.

For native builds, see the `build-native.sh` script.

## Repositories

- <https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git> -
  the URL of the xPack build scripts repository
- <https://github.com/xpack-dev-tools/build-helper> - the URL of the
  xPack build helper, used as the `scripts/helper` submodule

The build scripts use the same source code as Arm.

### Branches

- `xpack` - the updated content, used during builds
- `xpack-develop` - the updated content, used during development
- `master` - empty, not used.

## Prerequisites

The prerequisites are common to all binary builds. Please follow the
instructions in the separate
[Prerequisites for building binaries](https://xpack.github.io/xbb/prerequisites/)
page and return when ready.

Note: Building the Arm binaries requires an Arm machine.

## Download the build scripts repo

The build scripts are available in the `scripts` folder of the
[`xpack-dev-tools/aarch64-none-elf-gcc-xpack`](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack)
Git repo.

To download them, issue the following commands:

```sh
rm -rf ${HOME}/Work/aarch64-none-elf-gcc-xpack.git; \
git clone \
  https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git \
  ${HOME}/Work/aarch64-none-elf-gcc-xpack.git; \
git -C ${HOME}/Work/aarch64-none-elf-gcc-xpack.git submodule update --init --recursive
```

> Note: the repository uses submodules; for a successful build it is
> mandatory to recurse the submodules.

For development purposes, clone the `xpack-develop`
branch:

```sh
rm -rf ${HOME}/Work/aarch64-none-elf-gcc-xpack.git; \
git clone \
  --branch xpack-develop \
  https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git \
  ${HOME}/Work/aarch64-none-elf-gcc-xpack.git; \
git -C ${HOME}/Work/aarch64-none-elf-gcc-xpack.git submodule update --init --recursive
```

## The `Work` folder

The script creates a temporary build `Work/aarch64-none-elf-gcc-${version}`
folder in the user home. Although not recommended, if for any reasons
you need to change the location of the `Work` folder,
you can redefine `WORK_FOLDER_PATH` variable before invoking the script.

## Spaces in folder names

Due to the limitations of `make`, builds started in folders with
spaces in names are known to fail.

If on your system the work folder is in such a location, redefine it in a
folder without spaces and set the `WORK_FOLDER_PATH` variable before invoking
the script.

## Customizations

There are many other settings that can be redefined via
environment variables. If necessary,
place them in a file and pass it via `--env-file`. This file is
either passed to Docker or sourced to shell. The Docker syntax
**is not** identical to shell, so some files may
not be accepted by bash.

## Versioning

The version string is an extension to semver, the format looks like `11.2.1-1.2`.
It includes the three digits with the original GCC version, a fourth
digit with the Arm release, a fifth digit with the xPack release number.

When publishing on the **npmjs.com** server, a sixth digit is appended.

## Changes

Compared to the original Arm distribution, there should be no
functional changes.

The actual changes for each version are documented in the corresponding
release pages:

- <https://xpack.github.io/aarch64-none-elf-gcc/releases/>

## How to build local/native binaries

### README-DEVELOP.md

The details on how to prepare the development environment for native build
are in the
[`README-DEVELOP.md`](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/blob/xpack/README-DEVELOP.md) file.

## How to build distributions

## Build

The builds currently run on 5 dedicated machines (Intel GNU/Linux,
Arm 32 GNU/Linux, Arm 64 GNU/Linux, Intel macOS and Arm macOS.

### Build the Intel GNU/Linux and Windows binaries

The current platform for Intel GNU/Linux and Windows production builds is a
Debian 10, running on an Intel NUC8i7BEH mini PC with 32 GB of RAM
and 512 GB of fast M.2 SSD. The machine name is `xbbli`.

```sh
caffeinate ssh xbbli
```

Before starting a build, check if Docker is started:

```sh
docker info
```

Before running a build for the first time, it is recommended to preload the
docker images.

```sh
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh preload-images
```

The result should look similar to:

```console
$ docker images
REPOSITORY       TAG                    IMAGE ID       CREATED         SIZE
ilegeul/ubuntu   amd64-18.04-xbb-v3.4   ace5ae2e98e5   4 weeks ago     5.11GB
```

It is also recommended to Remove unused Docker space. This is mostly useful
after failed builds, during development, when dangling images may be left
by Docker.

To check the content of a Docker image:

```sh
docker run --interactive --tty ilegeul/ubuntu:amd64-18.04-xbb-v3.4
```

To remove unused files:

```sh
docker system prune --force
```

Since the build takes a while, use `screen` to isolate the build session
from unexpected events, like a broken
network connection or a computer entering sleep.

```sh
screen -S arm

sudo rm -rf ~/Work/aarch64-none-elf-gcc-*-*
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh --develop --linux64 --win64
```

or, for development builds:

```sh
sudo rm -rf ~/Work/aarch64-none-elf-gcc-*-*
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh --develop --disable-tests --linux64 --win64
```

When ready, run the build on the production machine (`xbbli`):

To detach from the session, use `Ctrl-a` `Ctrl-d`; to reattach use
`screen -r arm`; to kill the session use `Ctrl-a` `Ctrl-k` and confirm.

About 1 hour later, the output of the build script is a set of 4 files and
their SHA signatures, created in the `deploy` folder:

```console
$ ls -l ~/Work/aarch64-none-elf-gcc-*/deploy
total 249352
-rw-rw-rw- 1 ilg ilg 121050450 May 15 08:40 xpack-aarch64-none-elf-gcc-11.2.1-1.2-linux-x64.tar.gz
-rw-rw-rw- 1 ilg ilg       121 May 15 08:40 xpack-aarch64-none-elf-gcc-11.2.1-1.2-linux-x64.tar.gz.sha
-rw-rw-rw- 1 ilg ilg 134272873 May 15 08:57 xpack-aarch64-none-elf-gcc-11.2.1-1.2-win32-x64.zip
-rw-rw-rw- 1 ilg ilg       118 May 15 08:57 xpack-aarch64-none-elf-gcc-11.2.1-1.2-win32-x64.zip.sha
```

### Build the Arm GNU/Linux binaries

The supported Arm architectures are:

- `armhf` for 32-bit devices
- `aarch64` for 64-bit devices

The current platform for Arm GNU/Linux production builds is Raspberry Pi OS,
running on a pair of Raspberry Pi4s, for separate 64/32 binaries.
The machine names are `xbbla64` and `xbbla32`.

```sh
caffeinate ssh xbbla64
caffeinate ssh xbbla32
```

Before starting a multi-platform build, check if Docker is started:

```sh
docker info
```

Before running a build for the first time, it is recommended to preload the
docker images.

```sh
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh preload-images
```

The result should look similar to:

```console
$ docker images
REPOSITORY       TAG                      IMAGE ID       CREATED          SIZE
hello-world      latest                   46331d942d63   6 weeks ago     9.14kB
ilegeul/ubuntu   arm64v8-18.04-xbb-v3.4   4e7f14f6c886   4 months ago    3.29GB
ilegeul/ubuntu   arm32v7-18.04-xbb-v3.4   a3718a8e6d0f   4 months ago    2.92GB
```

Since the build takes a while, use `screen` to isolate the build session
from unexpected events, like a broken
network connection or a computer entering sleep.

```sh
screen -S arm

sudo rm -rf ~/Work/aarch64-none-elf-gcc-*-*
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh --develop --arm64
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh --develop --arm32
```

or, for development builds:

```sh
screen -S arm

sudo rm -rf ~/Work/aarch64-none-elf-gcc-*-*
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh --develop --disable-tests --arm64
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh --develop --disable-tests --arm32
```

To detach from the session, use `Ctrl-a` `Ctrl-d`; to reattach use
`screen -r arm`; to kill the session use `Ctrl-a` `Ctrl-k` and confirm.

About 13-14 hours later, the output of the build script is a set of 2
archives and their SHA signatures, created in the `deploy` folder:

```console
$ ls -l ~/Work/aarch64-none-elf-gcc-*/deploy
total 116500
-rw-rw-rw- 1 ilg ilg 119291455 May 15 10:50 xpack-aarch64-none-elf-gcc-11.2.1-1.2-linux-arm64.tar.gz
-rw-rw-rw- 1 ilg ilg       123 May 15 10:50 xpack-aarch64-none-elf-gcc-11.2.1-1.2-linux-arm64.tar.gz.sha
```

and:

```console
$ ls -l ~/Work/aarch64-none-elf-gcc-*/deploy
total 110880
-rw-rw-rw- 1 ilg ilg 113528873 May 15 10:59 xpack-aarch64-none-elf-gcc-11.2.1-1.2-linux-arm.tar.gz
-rw-rw-rw- 1 ilg ilg       121 May 15 10:59 xpack-aarch64-none-elf-gcc-11.2.1-1.2-linux-arm.tar.gz.sha
```

### Build the macOS binaries

The current platforms for macOS production builds are:

- a macOS 10.13.6 running on a MacBook Pro 2011 with 32 GB of RAM and
  a fast SSD; the machine name is `xbbmi`
- a macOS 11.6.1 running on a Mac Mini M1 2020 with 16 GB of RAM;
  the machine name is `xbbma`

```sh
caffeinate ssh xbbmi
caffeinate ssh xbbma
```

To build the latest macOS version:

```sh
screen -S arm

sudo rm -rf ~/Work/aarch64-none-elf-gcc-*-*
caffeinate bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh --develop --macos
```

or, for development builds:

```sh
sudo rm -rf ~/Work/aarch64-none-elf-gcc-*-*
caffeinate bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh --develop --disable-tests --macos
```

To detach from the session, use `Ctrl-a` `Ctrl-d`; to reattach use
`screen -r arm`; to kill the session use `Ctrl-a` `Ctrl-k` and confirm.

In about 2 hours, the output of the build script is a compressed archive
and its SHA signature, created in the `deploy` folder:

```console
$ ls -l ~/Work/aarch64-none-elf-gcc-*/deploy
total 230152
-rw-r--r--  1 ilg  staff  116660473 May 15 09:57 xpack-aarch64-none-elf-gcc-11.2.1-1.2-darwin-x64.tar.gz
-rw-r--r--  1 ilg  staff        122 May 15 09:57 xpack-aarch64-none-elf-gcc-11.2.1-1.2-darwin-x64.tar.gz.sha
```

and:

```console
$ ls -l ~/Work/aarch64-none-elf-gcc-*/deploy
total 227064
-rw-r--r--  1 ilg  staff  116250268 May 15 08:35 xpack-aarch64-none-elf-gcc-11.2.1-1.2-darwin-arm64.tar.gz
-rw-r--r--  1 ilg  staff        124 May 15 08:35 xpack-aarch64-none-elf-gcc-11.2.1-1.2-darwin-arm64.tar.gz.sha
```

## Subsequent runs

### Separate platform specific builds

Instead of `--all`, you can use any combination of:

```console
--linux64 --win64
```

On Arm, instead of `--all`, you can use any combination of:

```console
--arm64 --arm32
```

Please note that, due to the specifics of the GCC build process, the
Windows build requires the corresponding GNU/Linux build, so `--win64`
should be run after or together with `--linux64`.

### clean

To remove most build files, use:

```sh
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh clean
```

To also remove the repository and the output files, use:

```sh
bash ${HOME}/Work/aarch64-none-elf-gcc-xpack.git/scripts/helper/build.sh cleanall
```

For production builds it is recommended to completely remove the build folder.

### --develop

For performance reasons, the actual build folders are internal to each
Docker run, and are not persistent. This gives the best speed, but has
the disadvantage that interrupted builds cannot be resumed.

For development builds, it is possible to define the build folders in the
host file system, and resume an interrupted build.

### --debug

For development builds, it is also possible to create everything
with `-g -O0` and be able to run debug sessions.

### --jobs

By default, the build steps use all available cores. If, for any reason,
parallel builds fail, it is possible to reduce the load.

### --disable-multilib

For development builds, to save time, it is recommended to build the
toolchain without multilib.

### Interrupted builds

The Docker scripts run with root privileges. This is generally not a
problem, since at the end of the script the output files are reassigned
to the actual user.

However, for an interrupted build, this step is skipped, and files in
the install folder will remain owned by root. Thus, before removing the
build folder, it might be necessary to run a recursive `chown`.

## Testing

A simple test is performed by the script at the end, by launching the
executables to check if all shared/dynamic libraries are correctly used.

For a true test you need to unpack the archive in a temporary location
(like `~/Downloads`) and then run the
program from there. For example on macOS the output should
look like:

```console
$ .../Downloads/xpack-aarch64-none-elf-gcc-11.2.1-1.2/bin/aarch64-none-elf-gcc --version
aarch64-none-elf-gcc (xPack GNU Aarch64 Embedded GCC x86_64) 11.2.1 20220111
...
```

## Installed folders

After install, the package should create a structure like this (only the
first two depth levels are shown):

```console
$ tree -L 2 /Users/ilg/Library/xPacks/\@xpack-dev-tools/aarch64-none-elf-gcc/11.2.1-1.2/.content/
/Users/ilg/Library/xPacks/\@xpack-dev-tools/aarch64-none-elf-gcc/11.2.1-1.2/.content/
├── README.md
├── aarch64-none-elf
│   ├── bin
│   ├── include
│   ├── lib
│   └── share
├── bin
│   ├── aarch64-none-elf-addr2line
│   ├── aarch64-none-elf-ar
│   ├── aarch64-none-elf-as
│   ├── aarch64-none-elf-as-py3
│   ├── aarch64-none-elf-c++
│   ├── aarch64-none-elf-c++filt
│   ├── aarch64-none-elf-cpp
│   ├── aarch64-none-elf-elfedit
│   ├── aarch64-none-elf-g++
│   ├── aarch64-none-elf-gcc
│   ├── aarch64-none-elf-gcc-11.2.1
│   ├── aarch64-none-elf-gcc-ar
│   ├── aarch64-none-elf-gcc-nm
│   ├── aarch64-none-elf-gcc-ranlib
│   ├── aarch64-none-elf-gcov
│   ├── aarch64-none-elf-gcov-dump
│   ├── aarch64-none-elf-gcov-tool
│   ├── aarch64-none-elf-gdb
│   ├── aarch64-none-elf-gdb-add-index
│   ├── aarch64-none-elf-gdb-add-index-py3
│   ├── aarch64-none-elf-gdb-py3
│   ├── aarch64-none-elf-gfortran
│   ├── aarch64-none-elf-gprof
│   ├── aarch64-none-elf-gprof-py3
│   ├── aarch64-none-elf-ld
│   ├── aarch64-none-elf-ld.bfd
│   ├── aarch64-none-elf-lto-dump
│   ├── aarch64-none-elf-nm
│   ├── aarch64-none-elf-objcopy
│   ├── aarch64-none-elf-objdump
│   ├── aarch64-none-elf-ranlib
│   ├── aarch64-none-elf-readelf
│   ├── aarch64-none-elf-size
│   ├── aarch64-none-elf-strings
│   └── aarch64-none-elf-strip
├── distro-info
│   ├── CHANGELOG.md
│   ├── licenses
│   ├── patches
│   └── scripts
├── include
│   └── gdb
├── lib
│   ├── bfd-plugins
│   ├── gcc
│   ├── libcc1.0.so
│   ├── libcc1.so -> libcc1.0.so
│   └── python3.10
├── libexec
│   ├── gcc
│   ├── libcrypt.2.dylib
│   ├── libcrypto.1.1.dylib
│   ├── libffi.8.dylib
│   ├── libgcc_s.1.dylib
│   ├── libgmp.10.dylib
│   ├── libiconv.2.dylib
│   ├── libisl.15.dylib
│   ├── liblzma.5.dylib
│   ├── libmpc.3.dylib
│   ├── libmpfr.4.dylib
│   ├── libncurses.6.dylib
│   ├── libpanel.6.dylib
│   ├── libpython3.10.dylib
│   ├── libreadline.8.1.dylib
│   ├── libreadline.8.dylib -> libreadline.8.1.dylib
│   ├── libsqlite3.0.dylib
│   ├── libssl.1.1.dylib
│   ├── libstdc++.6.dylib
│   ├── libz.1.2.12.dylib
│   └── libz.1.dylib -> libz.1.2.12.dylib
└── share
    ├── doc
    └── gcc-11.2.1

21 directories, 59 files
```

No other files are installed in any system folders or other locations.

## Uninstall

The binaries are distributed as portable archives; thus they do not
need to run a setup and do not require an uninstall.

## Files cache

The XBB build scripts use a local cache such that files are downloaded only
during the first run, later runs being able to use the cached files.

However, occasionally some servers may not be available, and the builds
may fail.

The workaround is to manually download the files from an alternate
location (like
<https://github.com/xpack-dev-tools/files-cache/tree/master/libs>),
place them in the XBB cache (`Work/cache`) and restart the build.

## More build details

The build process is split into several scripts. The build starts on the
host, with `build.sh`, which runs `container-build.sh` several times,
once for each target, in one of the two docker containers. Both scripts
include several other helper scripts. The entire process is quite complex,
and an attempt to explain its functionality in a few words would not
be realistic. Thus, the authoritative source of details remains the source
code.
