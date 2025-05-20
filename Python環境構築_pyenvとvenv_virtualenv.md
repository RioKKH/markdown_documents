# Python環境構築

## pyenvとvenv/virtualenv

### ”externally-managed-environment”エラーが出る場合の対応

最近のUbuntu + Python3.12で発生するようになったエラー。

1. 仮想環境(venv, virtualenv, pyenv-virtualenv)を用いる

   仮想環境を作成したうえで、その中でpip installすることでほぼ解決できる

   1. pyenvとpyenv-virtualenvの両方をインストールする
      ```bash
      $ pyenv install 3.13.2
      $ pyenv virtualenv 3.13.2 autoware-3.13.2
      ```

   2. 作業ディレクトリで有効かする
      ```bash
      # 作業ディレクトリに移動する
      $ pyenv local autoware-3.13.2
      ```

   これで、ディレクトリ直下で使うpythonとpipは自作環境のものになる。

2. `python -m venv`を用いる場合
   シンプルなvenvでも可能

   ```bash
   $ pyenv install 3.13.2
   $ pyenv local 3.13.2 # 作業ディレクトリで実行する
   $ python -m venv venv # 同様に作業ディレクトリで実行する
   $ source venv/bin/activate
   ```

### 仮想環境削除方法

#### pyenv-virtualenvの場合

1. pyenv-virtualenvで作った仮想環境の削除
   ```bash
   $ pyenv uninstall autoware-3.13.2
   ```

2. 設定ファイルの削除・書き換え

   `pyenv local`で設定していたディレクトリの`.python-version`ファイルも、場合によっては削除または書き直す。

#### venvで作った仮想環境の削除

venvの場合、仮想環境の実態はディレクトリそのものなので、削除したい場合は、そのディレクトリ毎削除すればよいです。

```bash
# 仮想環境を.venvというディレクトリに作った場合
rm -rf .venv
```

### 仮想環境切り替方法

#### pyenv-virtualenvの場合

1. 今使っている仮想環境から抜ける
   ```bash
   $ deactivate
   ```

2. 別の環境に入る
   ```bash
   pyenv activate env2
   ```

一方で、pyenv localで自動切換えも可能です。もし作業ディレクトリに`.python-version`ファイルを置いている場合、そのディレクトリに移動するだけで、自動的にその環境に切り替わります。

#### venvの場合

1. 今使っている仮想環境から抜ける
   ```bash
   $ deactivate

2. 別の環境に入る
   ```bash
   $ source venv2/bin/activate
   ```

   ここで、別の仮想環境のディレクトリ名を`venv2`としています。