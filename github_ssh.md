# GitHub への SSH 設定

## 今からやることを手短に

### SSH ってなに？

リモートコンピュータと通信するためのプロトコル。

暗号通信できるので、盗聴のリスク無く通信できてうれしい

暗号通信には、公開鍵暗号というものを使ってる。

### じゃあ公開鍵暗号ってなに？？

公開鍵暗号とは「公開鍵」と「秘密鍵」という２つの鍵を使う暗号通信

公開鍵と秘密鍵はセットで生成される。名前の通り、公開鍵は他人に見せても良い鍵で、秘密鍵は他人に見せてはいけない鍵。そして、この公開鍵と秘密鍵が正しくマッチしたら、「正規の人がログインしようとしてるな」ってなって認証される

今回は GitHub に公開鍵を渡すことで、今後ターミナル上で push とかする時に「ほら秘密鍵もってるよ」っていうことで、GitHub へのアクセスを認めてもらうってことですね

## 手順

1. ホームディレクトリに移動する

   - Windows

   ```bash
   cd /Users/user_name
   ```

   `user_name`は、ユーザ名

   - Linux

   ```bash
   cd ~
   ```

2. `.ssh`ディレクトリに移動する

```bash
cd .ssh
```

3. 鍵を生成する

```bash
ssh-keygen -t rsa
```

`id_rsa`と`id_rsa.pub`が生成される。それぞれ秘密鍵と公開鍵を表す。

4. 公開鍵を GitHub に登録する。

   1. https://github.com/settings/keys にアクセス
   2. [New SSH Key]をクリック
   3. 適当な title を記入
   4. `id_rsa.pub`の中身を key にコピペ
   5. [Add SSH Key]をクリック

5. 接続テストをする

```
ssh -T git@github.com
```

Windows 版だと

```
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

と聞かれるので、yes と打って ENTER

```
Hi slime830! You've successfully authenticated, but GitHub does not provide shell access.
```

のように出力されたら成功
