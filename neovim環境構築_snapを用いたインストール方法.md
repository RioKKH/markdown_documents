# neovim環境構築

## snapを用いたインストール方法

### 1. snapを使ってダウンロード

```bash
$ snap info nvim # 現在利用可能なnvimを調べる
name:      nvim
summary:   Vim-fork focused on extensibility and usability
publisher: neovim-snap (neovim-snap)
store-url: https://snapcraft.io/nvim
contact:   justinkz+nvim@gmail.com
license:   Apache-2.0 OR Vim
description: |
  Neovim is a project that seeks to aggressively refactor Vim in order to:

  - Simplify maintenance and encourage contributions
  - Split the work between multiple developers
  - Enable the implementation of new/modern user interfaces without any modifications to the core
  source
  - Improve extensibility with a new plugin architecture
snap-id: iCEzvDZMvRrIWd5XLxgff6Tc6Zx20aeO
channels:
  latest/stable:    v0.11.1                         2025-05-21 (3771) 33MB classic
  latest/candidate: ↑
  latest/beta:      ↑
  latest/edge:      v0.12.0-v0.11.0+427+g9784bc1346 2025-05-24 (3787) 33MB classic
```

現時点でのlatest/stableはv0.11.1であることが確認できたので、これをダウンロードする

```bash
$ snap download --stable nvim
# これによって以下の２つのファイルがダウンロードされる。
$ ls
nvim_3771.assert nvim_3771.snap
```

これをインストールするには以下のコマンドを実行する必要がある。
snapは２段階設計となっており、最初に署名を検証してパッケージの健全性を確保し、その後にインストールを行う形になっている。デフォルトでは`/snap/nvim`にインストールされる事になる。

```bash
$ snap ack nvim_3771.assert    # アサーション(書名情報)の承認
$ snap install nvim_3771.snap  # パッケージのインストール
```

### 2. 展開する

1. snapパッケージを手動で展開するには、`unsquashfs`コマンドを利用します。
   ```bash
   $ sudo apt install squashfs-tools
   ```

2. snapファイルの展開
   ```bash
   $ unsquashfs -d nvim0.11.1 nvim_3771.snap
   ```

   コマンドが実行されると現在の作業ディレクトリに`nvim0.11.1`というディレクトリが生成される。

3. update-alternativesの更新
   新しいバージョンを**alternatives**に追加する。

   ```bash
   $ sudo update-alternatives --install /usr/bin/vi vi /home/rio/bin/nvim0.11.1/usr/bin/nvim 100
   $ sudo update-alternatives --install /usr/bin/vim vim /home/rio/bin/nvim0.11.1/usr/bin/nvim 100
   $ sudo update-alternatives --install /usr/bin/nvim nvim /home/rio/bin/nvim0.11.1/usr/bin/nvim 100
   ```

4. 使用するバージョンを選択する。

   ```bash
   $ sudo update-alternatives --config vi
   ```

5. バージョンを確認する
   ```bash
   $ nvim --version
   ```

   

