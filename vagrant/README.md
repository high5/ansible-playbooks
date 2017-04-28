## ssh_config について
`~/.ssh/config`とは別に`ssh_config`を作成している  
`ansible.cfg`に  
```
[ssh_connection]
ssh_args = -F ssh_config
```
と設定してやることで`ssh_config`の設定にあるhostにつなぐことが可能である

## 対象サーバー
local の vagrant を対象にしている  
ssh_configにvagrantの接続情報を記入してhostsにdefault を指定している

## inventory ファイル
`inventory`ファイルは接続するhostを定義するファイルで今回は一応、本番・ステージング・開発で環境毎にわけている。  
```
├── hosts
│   ├── dev
│   │   └── inventory
│   ├── stg
│   │   └── inventory
│   └── prd
│       └── inventory
```  

`-i hosts/prd` と inventoryの指定オプション `-i` の引数に環境用のフォルダ名をわたしてやれば中のinventoryファイルを勝手に読みこんでくれる。  
ファイル　`inventory`を用意するのを忘れずに　

## playbookを用意する
お試し用に`ping`を実行する playbookである`ping.yml`を用意した  
```
---
- hosts: all #対象を指定。allは全ホスト
  remote_user: vagrant
  tasks: #実行するtaskを以下に指定
   - name: pingしてみる #taskの名前
     ping:
```
以下のコマンドでpingの応答がかえってくる  
```
$ ansible-playbook ping.yml -i hosts/prd
```
コマンドの引数の順番は下記みたいな感じでもOK
```
$ ansible-playbook -i hosts/prd ping.yml
```
