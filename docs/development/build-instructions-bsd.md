# Build Instructions (BSD)

Follow the guidelines below for building Electron on \*BSD.

## Prerequisites

* At least 25GB disk space and 8GB RAM.
* Python 2.7.x. 
* Node.js.
* Clang 3.9.
* Development headers of GTK+ and libnotify.
* libchromiumcontent

There's no prebuild binaries for \*BSD, so you sould build libchromiumcontent first.
See [libchromiumcontent] for details. If official repo is not updated yet, access
[alternative repo].

After building [libchromiumcontent], run following command to install remain dependencies.
```bash
# pkg install libnotify
```

## Getting the Code

```bash
$ git clone https://github.com/electron/electron.git
```

If official repo is not updated yet, you may need use alternative repo:
https://github.com/jinchizhong/electron.git

## Bootstrapping

After building [libchromiumcontent], use following command to configure electron:

```bash
$ cd electron
$ CC_SRC=/path/to/libchromiumcontent/src
$ ./script/bootstrap.py -v --libcc_source_path "$CC_SRC" --libcc_shared_library_path "$CC_SRC/out-x64/shared_library" --libcc_static_library_path "$CC_SRC/out-x64/shared_library"
```

## Building

If you would like to build both `Release` and `Debug` targets:

```bash
$ ./script/build.py
```

This script will cause a very large Electron executable to be placed in
the directory `out/R`. The file size is in excess of 1.3 gigabytes. This
happens because the Release target binary contains debugging symbols.
To reduce the file size, run the `create-dist.py` script:

```bash
$ ./script/create-dist.py
```

This will put a working distribution with much smaller file sizes in
the `dist` directory. After running the create-dist.py script, you
may want to remove the 1.3+ gigabyte binary which is still in `out/R`.

You can also build the `Debug` target only:

```bash
$ ./script/build.py -c D
```

After building is done, you can find the `electron` debug binary under `out/D`.

## Cleaning

To clean the build files:

```bash
$ npm run clean
```

To clean only `out` and `dist` directories:

```bash
$ npm run clean-build
```

**Note:** Both clean commands require running `bootstrap` again before building.

## See Also

Building on Linux:
https://github.com/electron/electron/blob/master/docs/development/build-instructions-linux.md
