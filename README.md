# overview
ansibleでCentosにCAを簡易的に構築するサンプル  
※諸事情でCA部分だけ切り抜いただけなので、未検証

+ CA
+ サーバ証明穂
+ クライアント証明書
+ 失効リスト

## 実行方法
ローカルへの実行方法

```bash
# 初期構築
ansible-playbook -i localhost, -c local playbook.yml --extra-vars="ca_init=true"
```

## コマンド詳細（docker対象）

```bash
# 認証局を構築してCA秘密鍵とルート証明書を再作成
ansible-playbook -i ./docker_inventory ./docker_playbook.yml --tags ca --extra-vars="ca_init=true"
# apache用にサーバ証明書を作成
ansible-playbook -i ./docker_inventory ./docker_playbook.yml --tags ca --extra-vars="create_server_crt=true"
# クライアント証明書を作成
ansible-playbook -i ./docker_inventory ./docker_playbook.yml --tags ca --extra-vars="create_client_crt=true"
# 失効リストを作成
ansible-playbook -i ./docker_inventory ./docker_playbook.yml --tags ca --extra-vars="create_crl=true"
```

※`ca_init`をtrueにした場合はサーバ証明書、クライアント証明、失効リストも再作成される

## dockerに対する実行
初期構築はdockerで行った

* コンテナはchuukei_test/docker-compose.ymlで作成
* dockerコンテナに対する実行は`ansible-playbook -i docker_inventory docker_playbook.yml`

