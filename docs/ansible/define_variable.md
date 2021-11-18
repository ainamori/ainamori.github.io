# Ansible で変数が定義済み時のみtaskが実行されるようにする

## キーワード

- Ansible
 - variable
 - include_task
 - when

## ターゲット

- ansible playbookを作成しようとしている人


## Version

- ansible-core 2.11, 2.12, 2.13

## 本文

ansible playbookを実行する際に引数を渡す事を前提とした物を書く際は以下の通りにする

- tasks/main.yml

```yaml

- include_tasks: task00.yml
  when: path is defined


```

- tasks/task00.yml

```yaml

- name: Checking variable
  ansible.builtin.uri:
    url: "https://example.com/{{ path }}"
    method: GET
```

## まとめ

最初、以下の2つぐらいやり方を考えたがこれが一番シンプルで良い

- task毎にwhenを書く
  - 後続のタスクが依存するのでregister等を都度列挙して面倒くさい
- 最初に変数チェッカーtaskを入れる。
  - 変数が増える度に変数チェッカーへの追記が必要になる
  - rolesで作業を分割する為、一括に全部チェックを作ると使い回しが聞かない

