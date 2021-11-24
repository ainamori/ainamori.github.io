# Ansible で変数が定義済み時のみtaskが実行されるようにする

## キーワード

- Ansible
 - variable

## ターゲット

- ansible playbookを作成しようとしている人


## Version

- ansible-core 2.11, 2.12, 2.13

## 本文

Ansible で変数を見やすく整形するには `string | to_nice_XXX` を使うと良い。


## まとめ

- [Ansible filter](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html)