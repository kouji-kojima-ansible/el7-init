el7-init
=========

locale=LANG=ja_JP.utf8, timzone=Asia/Tokyo, proxy, 社内CA証明書を登録し、SELinux,firewalldを無効にするロール。
Role to set locale=LANG=ja_JP.utf8, timzone=Asia/Tokyo, proxy and register Internal CA and disable SELinux and firewalld.


Requirements
------------

AWS EC2 にて CentOS 7.x, RHEL 7.x を作成したところから始めてください。
Please start from the point where you created CentOS 7.x, RHEL 7.x with AWS EC2.


Dependencies
------------

無し(None)


Example Playbook
----------------

以下のように site.yml を 作成してください。
Please create site.yml as follows.

```
cat << EOF > site.yml
- hosts: servers
  remote_user: ec2-user
  sudo: yes
  vars:
    proxy_host: proxy.xxxxxxxxx.co.jp
    proxy_port: port_no
    no_proxys: xxxxx.co.jp,yyyy.co.jp
    ca_url: https://xxxxxxxx.co.jp/xxx.ca
    ca_sha256: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx(*1)
  roles:
    - { role: kouji-kojima-ansible.el7-init }
EOF
```

*1. xxx.ca の ca_sha256 の 値は sha256sum コマンドで確認してください。
    Please check the value of xxx.ca's ca_sha256 with the sha256sum command.

```
sha256sum xxx.ca
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx(Hash Value)
```


Example Hosts
-------------

```
# localhostの場合(In case of localhost)
cat << EOF > localhost
[servers]
localhost ansible_connection=local
EOF

# ステージング環境の場合(In case of staging environment)
cat << EOF > staging
[servers]
HostName or IP
HostName or IP

[all:vars]
ansible_ssh_user=ec2-user
EOF

# 本番環境の場合(In case of production environment)
cat << EOF > production
[servers]
HostName or IP
HostName or IP

[all:vars]
ansible_ssh_user=ec2-user
EOF
```


Execute Playbook
-----------------

実行例(Normal execution)

```
# ローカルの場合(In case of localhost)
ansible-playbook -i localhost site.yml --private-key=/path/key.pem

# ステージング環境の場合(In case of staging environment)
ansible-playbook -i staging site.yml --private-key=/path/key.pem

# 本番環境の場合(In case of production environment)
ansible-playbook -i production site.yml --private-key=/path/key.pem
```

デバッグ実行例(Debug execution)

```
# ローカルの場合(In case of localhost)
ansible-playbook -i localhost site.yml --private-key=/path/key.pem -vvv

# ステージング環境の場合(In case of staging environment)
ansible-playbook -i staging site.yml --private-key=/path/key.pem -vvv

# 本番環境の場合(In case of production environment)
ansible-playbook -i production site.yml --private-key=/path/key.pem -vvv
```


License
-------

[Apache License Version 2.0](https://github.com/kouji-kojima-ansible/el7-init/blob/master/LICENSE)


Author Information
------------------

Kouji Kojima
