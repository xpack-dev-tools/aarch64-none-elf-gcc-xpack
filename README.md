[![GitHub package.json version](https://img.shields.io/github/package-json/v/xpack-dev-tools/aarch64-none-elf-gcc-xpack)](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/blob/xpack/package.json)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/xpack-dev-tools/aarch64-none-elf-gcc-xpack)](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/releases/)
[![npm (scoped)](https://img.shields.io/npm/v/@xpack-dev-tools/aarch64-none-elf-gcc.svg?color=blue)](https://www.npmjs.com/package/@xpack-dev-tools/aarch64-none-elf-gcc/)
[![license](https://img.shields.io/github/license/xpack-dev-tools/aarch64-none-elf-gcc-xpack)](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/blob/xpack/LICENSE)

# The xPack GNU AArch64 Embedded GCC

A standalone cross-platform (Windows/macOS/Linux) **GNU AArch64 Embedded GCC**
binary distribution, intended for reproducible builds.

In addition to the the binary archives and the package meta data,
this project also includes the build scripts.

## Overview

This open source project is hosted on GitHub as
[`xpack-dev-tools/aarch64-none-elf-gcc-xpack`](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack)
and provides the platform specific binaries for the
[xPack GNU AArch64 Embedded GCC](https://xpack.github.io/aarch64-none-elf-gcc/).

The binaries can be installed automatically as **binary xPacks** or manually as
**portable archives**.

## Release schedule

This distribution plans to follow the official
[Arm GNU Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/downloads/)
distribution, by Arm.

## User info

This section is intended as a shortcut for those who plan
to use the GNU AArch64 Embedded GCC binaries. For full details please read the
[xPack GNU AArch64 Embedded GCC](https://xpack.github.io/aarch64-none-elf-gcc/) pages.

### Easy install

The easiest way to install GNU AArch64 Embedded GCC is using the **binary xPack**, available as
[`@xpack-dev-tools/aarch64-none-elf-gcc`](https://www.npmjs.com/package/@xpack-dev-tools/aarch64-none-elf-gcc)
from the [`npmjs.com`](https://www.npmjs.com) registry.

#### Prerequisites

A recent [xpm](https://xpack.github.io/xpm/),
which is a portable [Node.js](https://nodejs.org/) command line application.

It is recommended to update to the latest version with:

```sh
npm install --location=global xpm@latest
```

For details please follow the instructions in the
[xPack install](https://xpack.github.io/install/) page.

#### Install

With the `xpm` tool available, installing
the latest version of the package and adding it as
a dependency for a project is quite easy:

```sh
cd my-project
xpm init # Only at first use.

xpm install @xpack-dev-tools/aarch64-none-elf-gcc@latest

ls -l xpacks/.bin
```

This command will:

- install the latest available version,
into the central xPacks store, if not already there
- add symbolic links to the central store
(or `.cmd` forwarders on Windows) into
the local `xpacks/.bin` folder.

The central xPacks store is a platform dependent
folder; check the output of the `xpm` command for the actual
folder used on your platform).
This location is configurable via the environment variable
`XPACKS_STORE_FOLDER`; for more details please check the
[xpm folders](https://xpack.github.io/xpm/folders/) page.

For xPacks aware tools,
it is also possible to install GNU AArch64 Embedded GCC globally,
in the user home folder:

```sh
xpm install --global @xpack-dev-tools/aarch64-none-elf-gcc@latest
```

After install, the package should create a structure like this (macOS files;
only the first two depth levels are shown):

```console
$ tree -L 2 /Users/ilg/Library/xPacks/\@xpack-dev-tools/aarch64-none-elf-gcc/12.2.1-1.1/.content/
/Users/ilg/Library/xPacks/\@xpack-dev-tools/aarch64-none-elf-gcc/12.2.1-1.1/.content/
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
│   ├── aarch64-none-elf-gcc-12.2.1
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
    └── gcc-12.2.1

21 directories, 59 files
```

No other files are installed in any system folders or other locations.

#### Uninstall

To remove the links created by xpm in the current project:

```sh
cd my-project

xpm uninstall @xpack-dev-tools/aarch64-none-elf-gcc
```

To completely remove the package from the global store:

```sh
xpm uninstall --global @xpack-dev-tools/aarch64-none-elf-gcc
```

### Manual install

For all platforms, the **xPack GNU AArch64 Embedded GCC**
binaries are released as portable
archives that can be installed in any location.

The archives can be downloaded from the
GitHub [Releases](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/releases/)
page.

For more details please read the
[Install](https://xpack.github.io/aarch64-none-elf-gcc/install/) page.

### Versioning

The version strings used by the GCC project are three number strings
like `12.2.1`; to this string the xPack distribution adds a four number,
but since semver allows only three numbers, all additional ones can
be added only as pre-release strings, separated by a dash,
like `12.2.1-1`.
When published as a npm package, the version gets
a fifth number, like `12.2.1-1.1`.

Since adherence of third party packages to semver is not guaranteed,
it is recommended to use semver expressions like `^12.2.1` and `~12.2.1`
with caution, and prefer exact matches, like `12.2.1-1.1`.

## Maintainer info

For maintainer info, please see the
[README-MAINTAINER](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/blob/xpack/README-MAINTAINER.md)

## Support

The quick advice for getting support is to use the GitHub
[Discussions](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/discussions/).

For more details please read the
[Support](https://xpack.github.io/aarch64-none-elf-gcc/support/) page.

## License

The original content is released under the
[MIT License](https://opensource.org/licenses/MIT), with all rights
reserved to [Liviu Ionescu](https://github.com/ilg-ul/).

The binary distributions include several open-source components; the
corresponding licenses are available in the installed
`distro-info/licenses` folder.

## Download analytics

- GitHub [`xpack-dev-tools/aarch64-none-elf-gcc-xpack`](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/) repo
  - latest xPack release
[![Github All Releases](https://img.shields.io/github/downloads/xpack-dev-tools/aarch64-none-elf-gcc-xpack/latest/total.svg)](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/releases/)
  - all xPack releases [![Github All Releases](https://img.shields.io/github/downloads/xpack-dev-tools/aarch64-none-elf-gcc-xpack/total.svg)](https://github.com/xpack-dev-tools/aarch64-none-elf-gcc-xpack/releases/)
  - [individual file counters](https://somsubhra.github.io/github-release-stats/?username=xpack-dev-tools&repository=aarch64-none-elf-gcc-xpack) (grouped per release)
- npmjs.com [`@xpack-dev-tools/aarch64-none-elf-gcc`](https://www.npmjs.com/package/@xpack-dev-tools/aarch64-none-elf-gcc/) xPack
  - latest release, per month
[![npm (scoped)](https://img.shields.io/npm/v/@xpack-dev-tools/aarch64-none-elf-gcc.svg)](https://www.npmjs.com/package/@xpack-dev-tools/aarch64-none-elf-gcc/)
[![npm](https://img.shields.io/npm/dm/@xpack-dev-tools/aarch64-none-elf-gcc.svg)](https://www.npmjs.com/package/@xpack-dev-tools/aarch64-none-elf-gcc/)
  - all releases [![npm](https://img.shields.io/npm/dt/@xpack-dev-tools/aarch64-none-elf-gcc.svg)](https://www.npmjs.com/package/@xpack-dev-tools/aarch64-none-elf-gcc/)

Credit to [Shields IO](https://shields.io) for the badges and to
[Somsubhra/github-release-stats](https://github.com/Somsubhra/github-release-stats)
for the individual file counters.
