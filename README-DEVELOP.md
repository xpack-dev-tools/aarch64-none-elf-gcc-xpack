# README-DEVELOP

Developer notes.

## Arm branch

The Arm specific code is on a separate branch:

```sh
git remote add upstream git://gcc.gnu.org/git/gcc.git
git config --add remote.upstream.fetch "+refs/vendors/ARM/heads/*:refs/remotes/upstream/ARM/*"
git fetch upstream
```

## clang

As of 13.2, building with clang libc++ on Linux is currently problematic,
since the libtool definitions create shared libraries that use the gcc
libraries, while the compiler uses the libc++ libraries.

The result is an unwanted reference to libstdc++.so.6, spotted by the
post-processing scripts.

```console
libtool 2.4.7

/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/application/lib/gcc/aarch64-none-elf/13.2.1/plugin/libcc1plugin.so.0.0.0:
 0x000000000000000e (SONAME)             Library soname: [libcc1plugin.so.0]
 0x000000000000001d (RUNPATH)            Library runpath: [/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/lib:/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/clang/16:/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/x86_64-unknown-linux-gnu:/usr/lib/gcc/x86_64-linux-gnu/7:/lib/x86_64-linux-gnu:/lib64:/usr/lib/x86_64-linux-gnu:/lib:/usr/lib]
 0x0000000000000001 (NEEDED)             Shared library: [libunwind.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libpthread.so.0]
 0x0000000000000001 (NEEDED)             Shared library: [libstdc++.so.6] <---- !!!
 0x0000000000000001 (NEEDED)             Shared library: [libm.so.6]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.6]
 0x0000000000000001 (NEEDED)             Shared library: [libgcc_s.so.1]


/bin/bash ./libtool --tag=CXX   --mode=link /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/xpacks/.bin/clang++ -W -Wall  -fvisibility=hidden -fcf-protection  -ffunction-sections -fdata-sections -pipe -O2 -stdlib=libc++ -w -module -export-symbols /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/libcc1plugin.sym  '-L/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/lib' '-O2' '-v' '-stdlib=libc++' '-rtlib=compiler-rt' '-lunwind' -Xcompiler '-fuse-ld=lld' '-Wl,--gc-sections' '-L/home/ilg/.local/xPacks/@xpack-dev-tools/flex/2.6.4-1.1/.content/lib' '-lpthread' '-Wl,-rpath-link,/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/lib' '-Wl,-rpath,/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/lib' '-Wl,-rpath-link,/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/clang/16' '-Wl,-rpath,/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/clang/16' '-Wl,-rpath-link,/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/x86_64-unknown-linux-gnu' '-Wl,-rpath,/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/x86_64-unknown-linux-gnu' '-Wl,-rpath-link,/usr/lib/gcc/x86_64-linux-gnu/7' '-Wl,-rpath,/usr/lib/gcc/x86_64-linux-gnu/7' '-Wl,-rpath-link,/lib/x86_64-linux-gnu' '-Wl,-rpath,/lib/x86_64-linux-gnu' '-Wl,-rpath-link,/lib64' '-Wl,-rpath,/lib64' '-Wl,-rpath-link,/usr/lib/x86_64-linux-gnu' '-Wl,-rpath,/usr/lib/x86_64-linux-gnu' '-Wl,-rpath-link,/lib' '-Wl,-rpath,/lib' '-Wl,-rpath-link,/usr/lib' '-Wl,-rpath,/usr/lib' -o libcc1plugin.la -rpath /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/application/lib/gcc/aarch64-none-elf/13.2.1/plugin libcc1plugin.lo context.lo callbacks.lo connection.lo marshall.lo   -Wc,../libiberty/pic/libiberty.a

libtool: link: /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/xpacks/.bin/clang++  -fPIC -DPIC -shared -nostdlib /usr/lib/x86_64-linux-gnu/crti.o /usr/lib/gcc/x86_64-linux-gnu/7/crtbeginS.o  .libs/libcc1plugin.o .libs/context.o .libs/callbacks.o .libs/connection.o .libs/marshall.o   -L/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/lib -lunwind -L/home/ilg/.local/xPacks/@xpack-dev-tools/flex/2.6.4-1.1/.content/lib -lpthread -L/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/bin/../lib/x86_64-unknown-linux-gnu -L/usr/lib/gcc/x86_64-linux-gnu/7 -L/lib/x86_64-linux-gnu -L/lib/../lib64 -L/usr/lib/x86_64-linux-gnu -L/lib -L/usr/lib -lstdc++ -lm -lc -lgcc_s -lgcc /usr/lib/gcc/x86_64-linux-gnu/7/crtendS.o /usr/lib/x86_64-linux-gnu/crtn.o  -fuse-ld=lld -Wl,--gc-sections -Wl,-rpath-link -Wl,/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/lib -Wl,-rpath -Wl,/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/lib -Wl,-rpath-link -Wl,/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/clang/16 -Wl,-rpath -Wl,/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/clang/16 -Wl,-rpath-link -Wl,/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/x86_64-unknown-linux-gnu -Wl,-rpath -Wl,/home/ilg/.local/xPacks/@xpack-dev-tools/clang/16.0.6-1.1/.content/lib/x86_64-unknown-linux-gnu -Wl,-rpath-link -Wl,/usr/lib/gcc/x86_64-linux-gnu/7 -Wl,-rpath -Wl,/usr/lib/gcc/x86_64-linux-gnu/7 -Wl,-rpath-link -Wl,/lib/x86_64-linux-gnu -Wl,-rpath -Wl,/lib/x86_64-linux-gnu -Wl,-rpath-link -Wl,/lib64 -Wl,-rpath -Wl,/lib64 -Wl,-rpath-link -Wl,/usr/lib/x86_64-linux-gnu -Wl,-rpath -Wl,/usr/lib/x86_64-linux-gnu -Wl,-rpath-link -Wl,/lib -Wl,-rpath -Wl,/lib -Wl,-rpath-link -Wl,/usr/lib -Wl,-rpath -Wl,/usr/lib ../libiberty/pic/libiberty.a   -Wl,-soname -Wl,libcc1plugin.so.0 -Wl,-retain-symbols-file -Wl,/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/libcc1plugin.sym -o .libs/libcc1plugin.so.0.0.0


-stdlib=libc++ used for libcc1plugin.o

libtool: compile:  /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/xpacks/.bin/clang++ -DHAVE_CONFIG_H -I. -I/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1 -I /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/../include -I /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/../libgcc -I ../gcc -I/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/../gcc -I /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/../gcc/c -I/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/include -I /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/../gcc/c-family -I /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/../libcpp/include -I/home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/x86_64-pc-linux-gnu/install/include -I/home/ilg/.local/xPacks/@xpack-dev-tools/flex/2.6.4-1.1/.content/include -W -Wall -fvisibility=hidden -fcf-protection -ffunction-sections -fdata-sections -pipe -O2 -stdlib=libc++ -w -MT libcc1plugin.lo -MD -MP -MF .deps/libcc1plugin.Tpo -c /home/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/linux-x64/sources/gcc/libcc1/libcc1plugin.cc  -fPIC -DPIC -o .libs/libcc1plugin.o


libtool.m4:5524

      # Check if GNU C++ uses GNU ld as the underlying linker, since the
      # archiving commands below assume that GNU ld is being used.
      if test "$with_gnu_ld" = yes; then
        _LT_TAGVAR(archive_cmds, $1)='$CC $pic_flag -shared $libobjs $deplibs $compiler_flags ${wl}-soname $wl$soname -o $lib'        _LT_TAGVAR(archive_expsym_cmds, $1)='$CC $pic_flag -shared -nostdlib $predep_objects $libobjs $deplibs $postdep_objects $compiler_flags ${wl}-soname $wl$soname ${wl}-retain-symbols-file $wl$export_symbols -o $lib'
```

For the same plugin, when compiled on macOS, the link command is:

libtool: link: /Users/ilg/Work/xpack-dev-tools/aarch64-none-elf-gcc-xpack.git/build/darwin-x64/xpacks/.bin/clang++  -o .libs/libcc1plugin.0.so -bundle  .libs/libcc1plugin.o .libs/context.o .libs/callbacks.o .libs/connection.o .libs/marshall.o   -L/Users/ilg/Work/xpack-dev-tools-build/aarch64-none-elf-gcc-13.2.1-1.1/darwin-x64/x86_64-apple-darwin21.6.0/install/lib  -Wl,-undefined -Wl,dynamic_lookup -mmacosx-version-min=10.13 -Wl,-macosx_version_min -Wl,10.13 -Wl,-headerpad_max_install_names -Wl,-dead_strip -Wl,-rpath -Wl,/Users/ilg/Work/xpack-dev-tools-build/aarch64-none-elf-gcc-13.2.1-1.1/darwin-x64/x86_64-apple-darwin21.6.0/install/lib ../libiberty/pic/libiberty.a   -Wl,-exported_symbols_list,.libs/libcc1plugin-symbols.expsym
