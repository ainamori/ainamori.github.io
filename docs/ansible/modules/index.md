---
title: "Ansible modules index"
date:
draft: false
---
# Ansible Index

Ansible modules関連のページ

## Menu

### ansible.builtin modules

ansible-coreに含まれているため、何の心配も無く利用できる。

- [builtin.command](builtin_command.md)

### ansible.utils modules

utilsはcollectionという事になっている。
従って、`ansible-core`のみしかインストールしていない場合は、含まれない。
しかしながら通常であれば`pip install ansible` といった具合に`ansible`パッケージをインストールするかと思われるので
問題なく使えると思う。

- [utils.update_fact](utils.update_fact.md)
