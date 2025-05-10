# sudoコマンドでproxyをキープする方法

## 通常ユーザ用のproxy設定

先ずは通常ユーザー用のproxyを設定する。

npmコマンドで設定するのであれば

```bash
$ npm config set https-proxy http://プロキシサーバー名:ポート番号
# 認証が必要なプロキシの場合
$ npm config set proxy http://ユーザー名:パスワード@プロキシサーバー名:ポート番号
$ npm config set https-proxy http://ユーザー名:パスワード@プロキシサーバー名:ポート番号
```

環境変数を用いる場合は、.bashrcに以下のような関数を書いておいてもよい。

```bash
proxy() {
  local PROXY="proxy.nnnfff.co.jp"
  local PORT="8080"
  case "$1" in
  "on")
    export HTTP_PROXY="http://${PROXY}:${PORT}"
    export HTTPS_PROXY="http://${PROXY}:${PORT}"
    export FTP_PROXY="http://${PROXY}:${PORT}"
    export http_proxy="http://${PROXY}:${PORT}"
    export https_proxy="http://${PROXY}:${PORT}"
    export ftp_proxy="http://${PROXY}:${PORT}"
    ;;
  "off")
    unset HTTP_PROXY
    unset HTTPS_PROXY
    unset FTP_PROXY
    unset http_proxy
    unset https_proxy
    unset ftp_proxy
    ;;
  *)
    echo "Usage: proxy [on|off]"
    ;;
  esac
}
```

## sudoでproxyを使うための設定

```bash
$ sudo visudo
# 以下を追加する
Defaults env_keeps += "HTTP_PROXY HTTPS_PROXY http_proxy https_proxy"
```

上記設定の後、proxy設定を必要とするコマンドをsudoで実行してみる