---
title:  xPack GNU AArch64 Embedded GCC v13.3.1-1.1 released

date: 2024-08-15 03:12:52 +0300

authors: ilg-ul

# To be listed in the Releases page.
tags:
  - releases

# ----- Custom properties -----------------------------------------------------

arm_version: "13.3.Rel1"
arm_date: "4 July, 2024"
gcc_version: "13.3.1"
binutils_version: "2.42"
gdb_version: "14.2"
newlib_version: "4.4.0"
python_version: "3.11.4"

version: "13.3.1-1.1"
npm_subversion: "1"

download_url: https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/releases/tag/v13.3.1-1.1/

---

summary: "Version **13.3.1-1.1** is a new release; it follows the upstream Arm release."

<!-- truncate -->

import Image from '@theme/IdealImage';
import CodeBlock from '@theme/CodeBlock';

import Prerequisites from './_common/_prerequisites-glib-2.27.mdx'
import DeprecationNotices from './_common/_deprecation-notices-glib-2.27.mdx'
import DownloadAnalytics from './_common/_download-analytics.mdx'

The [xPack GNU AArch64 Embedded GCC](https://xpack.github.io/aarch64-none-elf-gcc/)
is a standalone cross-platform binary distribution of
[Arm GNU Toolchain](https://developer.arm.com/Tools%20and%20Software/GNU%20Toolchain).

There are separate binaries for **Windows** (x64),
**macOS** (x64 and arm64) and **GNU/Linux** (x64, arm64 and arm).

:::note Raspberry Pi

The main targets for the GNU/Linux Arm
binaries are the **Raspberry Pi** class devices (armv7l and aarch64;
armv6 is not supported).

:::

## Download

The binary files are available from <a href={frontMatter.download_url}>GitHub Releases</a>.

## Prerequisites

- x64 GNU/Linux: any system with **GLIBC 2.27** or higher
  (like Ubuntu 18 or later, Debian 10 or later, RedHat 8 or later,
  Fedora 29 or later, etc)
- arm64/arm GNU/Linux: any system with **GLIBC 2.27** or higher
  (like Raspberry Pi OS, Ubuntu 18 or later, Debian 10 or later, RedHat 8 or later,
  Fedora 29 or later, etc)
- x64 Windows: Windows 7 with the Universal C Runtime
  ([UCRT](https://support.microsoft.com/en-us/topic/update-for-universal-c-runtime-in-windows-c0514201-7fe6-95a3-b0a5-287930f3560c)),
  Windows 8, Windows 10
- x64 macOS: 10.13 or later
- arm64 macOS: 11.6 or later

## Install

The full details of installing the **xPack GNU AArch64 Embedded GCC** on various platforms
are presented in the [Install Guide](/docs/install/).

## Compliance

The xPack GNU AArch64 Embedded GCC generally follows the official
[Arm GNU Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/downloads/)
releases.

The current version is based on:

- [Arm GNU Toolchain](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads/)
release **{frontMatter.arm_version}** from {frontMatter.arm_date}
and uses the same sources. It includes:
  - GCC {frontMatter.gcc_version}
  - binutils {frontMatter.binutils_version}
  - newlib {frontMatter.newlib_version}
  - GDB {frontMatter.gdb_version}
  - Python {frontMatter.python_version}

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
support for **Python {frontMatter.python_version}**.

The Python 3 run-time is included, so GDB does not need any version of
Python to be installed, and is insensitive to the presence of other
versions.

### Text User Interface (TUI)

Support for TUI was added to GDB. The `ncurses` library was added to
the distribution.

:::note

TUI is not available on Windows

:::

### No Guile

Due to the difficulties of building standalone Guile libraries on all
platforms, support for Guile scripting in GDB is currently not available.

## Bug fixes

- none

## Enhancements

- none

## Known problems

- none

## Documentation

The original GNU GCC documentation is available
[online](https://gcc.gnu.org/onlinedocs/).

## Build

The binaries for all supported platforms
(Windows, macOS and GNU/Linux) were built using the
[xPack Build Box (XBB)](https://xpack.github.io/xbb/), a set
of build environments based on slightly older distributions, that should be
compatible with most recent systems.

For the prerequisites and more details on the build procedure, please see the
[Maintainer Info](/docs/maintainer/) page.

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

```txt
a9108be1446aa867876802c8e54f8d4f47fbd09c928fa2621e6e23fa366cbf13
xpack-aarch64-none-elf-gcc-13.3.1-1.1-darwin-arm64.tar.gz

11e01286b5a6dbcecdaf6dbd41b26104df45d2fdb05dbf5347ec9ae0caa288c4
xpack-aarch64-none-elf-gcc-13.3.1-1.1-darwin-x64.tar.gz

d1809b2442c1c7bb3d04f3b6b3b2e46ef94699a1faf76c4864b6470e6d583c84
xpack-aarch64-none-elf-gcc-13.3.1-1.1-linux-arm.tar.gz

68a9ac88508a82c2cba3e7ddf419d49a8b56062cd2f13cfab21887428bb4f980
xpack-aarch64-none-elf-gcc-13.3.1-1.1-linux-arm64.tar.gz

f76dc6d105f054fcb3f2a39ecf206d99101dc87931a5b9227fe886cb9478b667
xpack-aarch64-none-elf-gcc-13.3.1-1.1-linux-x64.tar.gz

47455ae3edd32924ce9a45bac5736bbcdd17161dd7288b61005d6fa5cdfb3952
xpack-aarch64-none-elf-gcc-13.3.1-1.1-win32-x64.zip

```

<DeprecationNotices/>

<DownloadAnalytics version={ frontMatter.version }/>
