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
ldpkg install foo.deb
```

# TODO
## 無限ループの回避
パッケージが相互に依存している場合がある．
(e.g. `openjdk-8-jre-headless`, `ca-certificates-java`)
インストール予定のものを記憶しておけばok．

## or での依存の場合
現状では，Depends が `hoge | fuga` のときは

1. `hoge` のインストールを試みる
2. それに失敗したら `fuga` のインストールを試みる

という手順になっているが，先に「どちらかがインストール済み」かをチェックした方が良さそう

## Recommends のインストール
Depends だけじゃなくて Recommends もインストールする

## message の改善
- インストール済みの場合にもいちいち `[Begin]` `[End]` と出るのは邪魔

## version もちゃんとチェックする
具体例(`libgmpxx4ldbl`)
- global には `2:5.1.3+dfsg-1ubuntu1` がインストール済み
- 実際に必要なのは `2:6.1.2+dfsg-2`

## apt-file
`apt-file` の Depends にある `perl:any` って何？

## 最初にインストール済みかどうかチェック
依存関係を確認/解決する前に，そもそもの目的のパッケージがインストール済みかどうか確認する

## Provides
- `.deb` ファイルにより provide されている仮想パッケージも，
  `ldpkg` 内の関数 `mark_installed` で記録する．
  `mark_installed` の引数は `.deb` ファイル名にすると良い？
- `is_installed_globally` でも仮想パッケージもチェックする．

## refactoring
`lapt__install_apt` がゴチャゴチャしてる．
