# TODO
## Recommends のインストール
Depends だけじゃなくて Recommends もインストールする

## version もちゃんとチェックする
具体例(`libgmpxx4ldbl`)
- global には `2:5.1.3+dfsg-1ubuntu1` がインストール済み
- 実際に必要なのは `2:6.1.2+dfsg-2`

## Provides
- `.deb` ファイルにより provide されている仮想パッケージも，
  `ldpkg` 内の関数 `mark_installed` で記録する．
  `mark_installed` の引数は `.deb` ファイル名にすると良い？
- `is_installed_globally` でも仮想パッケージもチェックする．
  ↑これは `apt-cache` に依存するから，`ldpkg` でやるのは変かも？

## 複数のパッケージ
`lapt install hoge fuga` で `hoge` と `fuga` を同時にインストール

# メモ
## .deb パッケージの名前に利用できる文字
[5. Control files and their fields — Debian Policy Manual v4.4.1.1](https://www.debian.org/doc/debian-policy/ch-controlfields.html#s-f-source)
の "5.6.1 Source" 内:

> Package names (both source and binary, see Package) must consist only of lower case letters (a-z), digits (0-9), plus (+) and minus (-) signs, and periods (.). They must be at least two characters long and must start with an alphanumeric character.
