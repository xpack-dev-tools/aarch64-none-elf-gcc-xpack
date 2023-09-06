---
title:  xPack GNU AArch64 Embedded GCC v{{ XBB_RELEASE_VERSION }} released

TODO: select one summary

summary: "Version **{{ XBB_RELEASE_VERSION }}** is a maintenance release; it fixes <...>."

summary: "Version **{{ XBB_RELEASE_VERSION }}** is a new release; it follows the upstream Arm release."

arm_version: "12.3.Rel1"
arm_date: "28 Jul, 2023"
gcc_version: "12.3.1"
binutils_version: "2.40"
gdb_version: "13.2"
newlib_version: "4.3.0"
python_version: "3.11.1"

version: "{{ XBB_RELEASE_VERSION }}"
npm_subversion: "1"

download_url: https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/releases/tag/v{{ XBB_RELEASE_VERSION }}/

comments: true

date:   {{ RELEASE_DATE }}

# For Jekyll releases selection.
categories:
  - releases
  - aarch64-none-elf-gcc

# For navigation; use scripts/createtag.sh in Jekyll.
tags:
  - releases
  - arm
  - aarch64-none-elf-gcc
  - gcc
  - binaries
  - c++
  - exceptions

---

[The xPack GNU AArch64 Embedded GCC](https://xpack.github.io/aarch64-none-elf-gcc/)
is a standalone cross-platform binary distribution of
[Arm GNU Toolchain](https://developer.arm.com/Tools%20and%20Software/GNU%20Toolchain).

There are separate binaries for **Windows** (Intel 64-bit),
**macOS** (Intel 64-bit, Apple Silicon 64-bit)
and **GNU/Linux** (Intel 64-bit, Arm 32/64-bit).

{% raw %}{% include note.html content="The main targets for the Arm binaries
are the **Raspberry Pi** class devices (armv7l and aarch64;
armv6 is not supported)." %}{% endraw %}

## Download

The binary files are available from GitHub [Releases]({% raw %}{{ page.download_url }}{% endraw %}).

## Prerequisites

- GNU/Linux Intel 64-bit: any system with **GLIBC 2.27** or higher
  (like Ubuntu 18 or later, Debian 10 or later, RedHat 8 later,
  Fedora 29 or later, etc)
- GNU/Linux Arm 32/64-bit: any system with **GLIBC 2.27** or higher
  (like Raspberry Pi OS, Ubuntu 18 or later, Debian 10 or later, RedHat 8 later,
  Fedora 29 or later, etc)
- Intel Windows 64-bit: Windows 7 with the Universal C Runtime
  ([UCRT](https://support.microsoft.com/en-us/topic/update-for-universal-c-runtime-in-windows-c0514201-7fe6-95a3-b0a5-287930f3560c)),
  Windows 8, Windows 10
- Intel macOS 64-bit: 10.13 or later
- Apple Silicon macOS 64-bit: 11.6 or later

## Install

The full details of installing the **xPack GNU AArch64 Embedded GCC** on various platforms
are presented in the separate
[Install]({% raw %}{{ site.baseurl }}{% endraw %}/dev-tools/aarch64-none-elf-gcc/install/) page.

### Easy install

The easiest way to install Arm Embedded GCC is with
[`xpm`]({% raw %}{{ site.baseurl }}{% endraw %}/xpm/)
by using the **binary xPack**, available as
[`@xpack-dev-tools/aarch64-none-elf-gcc`](https://www.npmjs.com/package/@xpack-dev-tools/aarch64-none-elf-gcc)
from the [`npmjs.com`](https://www.npmjs.com) registry.

With the `xpm` tool available, installing
the latest version of the package and adding it as
a development dependency for a project is quite easy:

```sh
cd my-project
xpm init # Add a package.json if not already present

xpm install @xpack-dev-tools/aarch64-none-elf-gcc@latest --verbose

ls -l xpacks/.bin
```

To install this specific version, use:

```sh
xpm install @xpack-dev-tools/aarch64-none-elf-gcc@{% raw %}{{ page.version }}.{{ page.npm_subversion }}{% endraw %} --verbose
```

For xPacks aware tools, like the **Eclipse Embedded C/C++ plug-ins**,
it is also possible to install Arm Embedded GCC globally, in the user home folder.

```sh
xpm install --global @xpack-dev-tools/aarch64-none-elf-gcc@latest --verbose
```

Eclipse will automatically
identify binaries installed with
`xpm` and provide a convenient method to manage paths.

### Uninstall

To remove the links created by xpm in the current project:

```sh
cd my-project

xpm uninstall @xpack-dev-tools/aarch64-none-elf-gcc
```

To completely remove the package from the central xPack store:

```sh
xpm uninstall --global @xpack-dev-tools/aarch64-none-elf-gcc
```

## Compliance

The xPack GNU AArch64 Embedded GCC generally follows the official
[Arm GNU Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/downloads/)
releases.

The current version is based on:

- [Arm GNU Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads/)
release **{% raw %}{{ page.arm_version }}{% endraw %}** from {% raw %}{{ page.arm_date }}{% endraw %}
and uses the same sources. It includes:
  - GCC {% raw %}{{ page.gcc_version }}{% endraw %}
  - binutils {% raw %}{{ page.binutils_version }}{% endraw %}
  - newlib {% raw %}{{ page.newlib_version }}{% endraw %}
  - GDB {% raw %}{{ page.gdb_version }}{% endraw %}

## Supported libraries

The supported libraries are:

```console
$ aarch64-none-elf-gcc -print-multi-lib
.;
ilp32;@mabi=ilp32
```

## Changes

Compared to the Arm version, there should be no functional changes.

### XML parsing in GDB

Some advanced GDB servers, like the one provided with SEGGER J-Link, are
capable of passing an XML with the target capabilities to the GDB client.
For unknown reasons, the Arm toolchain distribution came without XML
parsing support. The xPack distribution brings back support for
XML parsing and full integration with the SEGGER J-Link GDB server.

### Python

Support for Python scripting was added to GDB. This distribution provides
a separate binary, `aarch64-none-elf-gdb-py3` with
support for **Python {% raw %}{{ page.python_version }}{% endraw %}**.

The Python 3 run-time is included, so GDB does not need any version of
Python to be installed, and is insensitive to the presence of other
versions.

### Text User Interface (TUI)

Support for TUI was added to GDB. The `ncurses` library was added to
the distribution.

{% raw %}{% include note.html content="TUI is not available on Windows." %}{% endraw %}

### No Guile

Due to the difficulties of building standalone Guile libraries on all
platforms, support for Guile scripting in GDB is currently not available.

## Bug fixes

- none

## Enhancements

- none

## Known problems

- none

## Shared libraries

On all platforms the packages are standalone, and expect only the standard
runtime to be present on the host.

All dependencies that are build as shared libraries are copied locally
in the `libexec` folder (or in the same folder as the executable for Windows).

### `DT_RPATH` and `LD_LIBRARY_PATH`

On GNU/Linux the binaries are adjusted to use a relative path:

```console
$ readelf -d library.so | grep runpath
 0x000000000000001d (RPATH)            Library rpath: [$ORIGIN]
```

In the GNU ld.so search strategy, the `DT_RPATH` has
the highest priority, higher than `LD_LIBRARY_PATH`, so if this later one
is set in the environment, it should not interfere with the xPack binaries.

Please note that previous versions, up to mid-2020, used `DT_RUNPATH`, which
has a priority lower than `LD_LIBRARY_PATH`, and does not tolerate setting
it in the environment.

### `@rpath` and `@loader_path`

Similarly, on macOS, the binaries are adjusted with `install_name_tool` to use a
relative path.

## Documentation

The original GNU GCC documentation is available
[online](https://gcc.gnu.org/onlinedocs/).

## Build

The binaries for all supported platforms
(Windows, macOS and GNU/Linux) were built using the
[xPack Build Box (XBB)](https://xpack.github.io/xbb/), a set
of build environments based on slightly older distributions, that should be
compatible with most recent systems.

The scripts used to build this distribution are in:

- `distro-info/scripts`

For the prerequisites and more details on the build procedure, please see the
[How to build](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/blob/xpack/README-BUILD.md) page.

## CI tests

Before publishing, a set of simple tests were performed on an exhaustive
set of platforms. The results are available from:

- [GitHub Actions](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/actions/)
- [Travis CI](https://app.travis-ci.com/github/xpack-dev-tools/aarch64-none-elf-gcc-xpack/builds/)

## Tests

The binaries were tested on a variety of platforms,
but mainly to check the integrity of the
build, not the compiler functionality.

## Checksums

The SHA-256 hashes for the files are:
