# SSH Public Key Authentication

SSH公開鍵認証設定手順。

---

# 1. Client Side

## 1.1 ~/.ssh ディレクトリへ移動

```bash
cd ~/.ssh
```

存在しない場合は作成。

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

---

## 1.2 キーペア作成

```bash
ssh-keygen -t rsa -b 4096
```

以下ファイルが作成される。

```text
id_rsa
id_rsa.pub
```

---

## 1.3 公開鍵内容確認

```bash
cat ~/.ssh/id_rsa.pub
```

---

## 1.4 接続先サーバのホストキー登録

## 1.4 接続先サーバのホストキー登録

初回接続時、接続先サーバのホストキー確認メッセージが表示される。

```bash
ssh -o StrictHostKeyChecking=ask user@server
```

表示例：

```text
The authenticity of host 'server' can't be established.
ED25519 key fingerprint is SHA256:xxxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

`yes` を入力すると、
接続先サーバのホストキーがクライアント側の
`~/.ssh/known_hosts`
へ登録される。

```

登録後、以下ファイルに保存される。

```text
~/.ssh/known_hosts
```

---

# 2. Server Side

## 2.1 受信ユーザの ~/.ssh 作成
受信ユーザがなければ作成
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

---

## 2.2 authorized_keys 作成

```bash
vi ~/.ssh/authorized_keys
```

Client側の公開鍵を貼り付ける。

---

## 2.3 authorized_keys 権限設定

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

# 3. Connection Test

```bash
ssh user@server uanme
server(接続先のサーバのホスト名が表示される）
```

---

# Troubleshooting

## Permission denied (publickey)

確認項目：

- ~/.ssh 権限
- authorized_keys 権限（ユーザグループがrootになってしまい、あるいは権限が大きく設定され　600が無難）
- SELinux
- sshd_config
- 公開鍵内容誤り

---

# Notes

- 秘密鍵(id_rsa)は第三者へ共有しない
- 公開鍵(id_rsa.pub)のみサーバへ登録する
- rootログイン禁止推奨
