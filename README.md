# Building 

```bash
	cd build
	wget http://rsync.samba.org/ftp/rsync/src/rsync-3.1.1.tar.gz
	tar -zxvf rsync-3.1.1.tar.gz
	wget http://rsync.samba.org/ftp/rsync/src/rsync-patches-3.1.1.tar.gz
	tar -zxvf rsync-patches-3.1.1.tar.gz
	cd rsync-3.1.1
```

Apply patches

```bash
	patch -p1 < ../../Patches/rsync-3.1.1/decode_octal.patch
	patch -p1 < ../../Patches/rsync-3.1.1/ignore_apple_quarantine.patch
	patch -p1 < patches/ignore-case.diff
```

Build

```bash
	./configure CFLAGS='-arch x86_64 -arch i386 -mmacosx-version-min=10.8' LDFLAGS='-mmacosx-version-min=10.8'
	make
```

Check that it's a Mach'O universal binary (should have 2 architectures)

```bash
	file rsync
```

Check which shared libraries rsync is linked against

```bash
	otool -L rsync
```

Copy into xcode project.  We need to ensure that it is included in the build and signed automatically as part of the build

```bash
	cp rsync ~/Sources/dropsync/rsync_3.1.1 
```


# Creating a new patch

With new code in place issue a command like this

```bash
	diff -u original/io.c rsync-3.1.1/io.c > ../Patches/rsync-3.1.1/decode_octal.patch 
```

