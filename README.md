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
export CFLAGS="-I$LDPKG_DIR/usr/include"
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

# TODO
## Recommends のインストール
Depends だけじゃなくて Recommends もインストールする

## version もちゃんとチェックする
具体例(`libgmpxx4ldbl`)
- global には `2:5.1.3+dfsg-1ubuntu1` がインストール済み
- 実際に必要なのは `2:6.1.2+dfsg-2`

## apt-file
`apt-file` の Depends にある `perl:any` って何？

## Provides
- `.deb` ファイルにより provide されている仮想パッケージも，
  `ldpkg` 内の関数 `mark_installed` で記録する．
  `mark_installed` の引数は `.deb` ファイル名にすると良い？
- `is_installed_globally` でも仮想パッケージもチェックする．
  ↑これは `apt-cache` に依存するから，`ldpkg` でやるのは変かも？

# メモ
## .deb パッケージの名前に利用できる文字
[5. Control files and their fields — Debian Policy Manual v4.4.1.1](https://www.debian.org/doc/debian-policy/ch-controlfields.html#s-f-source)
の "5.6.1 Source" 内:

> Package names (both source and binary, see Package) must consist only of lower case letters (a-z), digits (0-9), plus (+) and minus (-) signs, and periods (.). They must be at least two characters long and must start with an alphanumeric character.
