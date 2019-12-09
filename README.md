You can install `.deb` packages without root access (i.e. without `sudo`).

- During installation, `lapt` resolves dependencies.
  But the versions of dependent packages are ignored.
- All packages are installed under your `$HOME` directory.
  By default, `$HOME/.ldpkg/` pretends to be the root directory `/`.
  This means that most libralies are installed to `$HOME/.ldpkg/usr/lib/`
  and most executable files are installed to `$HOME/.ldpkg/usr/bin/`.
- Targets of symbolic links and paths in `.pc` files (for pkgconfig) are edited
  so that they give appropriate files or directories under `$HOME/.ldpkg`.


# Requirements
- `apt` (and hence `dpkg`)
- `bash`

# Installation
```bash
cd /somewhere/in/path/
git clone git@github.com:shwaka/local-apt.git
ln -s local-apt/lapt lapt
ln -s local-apt/ldpkg ldpkg
```

Add the following lines to your `.bashrc` (or something similar).

```bash
export LDPKG_DIR=$HOME/.ldpkg
export PATH=$LDPKG_DIR/usr/bin:$PATH
export C_INCLUDE_PATH="$LDPKG_DIR/usr/include:$LDPKG_DIR/usr/include/x86_64-linux-gnu"
export CFLAGS="-I$LDPKG_DIR/usr/include -I$LDPKG_DIR/usr/include/x86_64-linux-gnu"
export CPPFLAGS=$CFLAGS
export LDFLAGS="-L$LDPKG_DIR/usr/lib -L$LDPKG_DIR/usr/lib/x86_64-linux-gnu"
export PKG_CONFIG_PATH="$LDPKG_DIR/usr/lib/x86_64-linux-gnu/pkgconfig:$LDPKG_DIR/usr/share/pkgconfig"
export LD_LIBRARY_PATH="$LDPKG_DIR/usr/lib:$LDPKG_DIR/usr/lib/x86_64-linux-gnu"
```

# Usage
```bash
lapt install <package-name>
lapt inatall -f <package-name>  # install even if the package is already installed (locally or globally)
lapt install -d foo.deb  # install dependent packages and then install foo.deb
lapt install -u <package-name>  # allow symlinks to be unfixed
ldpkg install foo.deb
ldpkg install -f foo.deb
```
