---
title: "Ansible modules community.docker"
date:
draft: false
---
# community.docker

## About this page.

Ansible で docker を操作するためのモジュール。
ただし、RedHatではコンテナ操作にはDocker.incのdockerではなくpodmanを推奨しているため、
)[podman_container_module](https://docs.ansible.com/ansible/latest/collections/containers/podman/podman_container_module.html)を使う方が正しいかもしれない。

2021.12現在でpodmanはまだまだdocker(-compose)に劣る部分も少なくないため、dockerを使っている人も多いと思われる。

## Requirements

- Ansible 実行ホスト
 - ansible-galaxy community.docker module.
- Ansible "Target" ホスト
 - docker
 - [Docker SDK for python](https://docker-py.readthedocs.io/en/stable/)

接続先でDockerが必要なのはなんとなく想像が付くと思われる。
だが、肝心なところはこの`Docker SDK for python`である。

Docker単体で利用する際には全く必要としない為、Ansibleで操作するために追加でインストールが必要なものになる。
困ったことにCentOS7等ではAnsibleが標準で参照するpython interpriterが2.7(/usr/bin/python or /usr/libexec/platform-python)の為、何も考えない場合は `yum install python-docker`辺りを実行してインストールすることになる。

### How to use.

ここはマニュアル通りに記載すれば動いてくれる





