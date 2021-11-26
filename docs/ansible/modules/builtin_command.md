---
title: "Ansible modules builtin.command"
date:
draft: false
---
# ansible.builtin.command

- https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html
  - source: https://github.com/ansible/ansible/blob/devel/lib/ansible/modules/command.py

## About this page.

Ansible 公式ドキュメントをみると、表に記載されたオプションと例示が全く合っていない。
従って何が正しい動作なのか調べるためにソースコードを見て確認した。
正しいExampleは以下の通り

### args vs argv vs cmd

exampleには`args`を使った表記と`argv`を使った物。および`cmd`を使った物がある。
例示と表記がブレているため、どのような違いがあるのかよくわからない。

ソースコードを追ったりドキュメントを読み込んだ結果、分かったことを以下に書く

### cmd

`cmd`は最も簡単な物なので初めに記す。
単に `ansible.builtin.command:`の後に書いている文字を分けて書けると言うだけ

以下は全く同じ動作をする。


```yaml
    - name: Run builtin.command by one-line
      ansible.builtin.command: "echo hoge"
```

```yaml
    - name: Run builtin.command with cmd
      ansible.builtin.command:
        cmd: "echo hoge"
```

### argv

`argv` は `cmd` と同じだが `argv`はコマンドをリストで受け取る
先の`cmd`と同じ結果を得ようとしたときの記述ルールは以下の通り

これはOK.

```yaml
    - name: Run builtin.command with argv as list.
      ansible.builtin.command:
        argv:
          - "echo"
          - "hoge"
```

これはNG.

```yaml
    - name: Run builtin.command with argv as string.
      ansible.builtin.command:
        argv: "echo hoge"
```


### args

`args` は `cmd`と`argv`とは全く異なる。 `args`にはdictだろうとlistだろうと
コマンドそのものを受け渡す物は無い。
`args`を使ってそのままコマンドを入れたい場合は以下の2通りの実行方法法がある


```yaml
    - name: Run builtin.command with args and cmd
      ansible.builtin.command:
      args:
        cmd: "echo hoge"
      ignore_errors: true
```


```yaml
    - name: Run builtin.command with args and argv
      ansible.builtin.command:
      args:
        argv:
          - "echo"
          - "hoge"
      ignore_errors: true
```

しかし、この方法では単に階層が深くなるだけで何らメリットが無い。
では、`args`は本来なんのためにあるのか。
それは適当な物を加えてエラーを出したときに分かる。

```yaml
    - name: Run builtin.command bad args
      ansible.builtin.command:
      args:
        free_form: "echo hoge"
      ignore_errors: true
```

```bash
TASK [Run builtin.command bad args] ***********************************************************************************************************************************************************************************
fatal: [localhost]: FAILED! => {"changed": false, "msg": "Unsupported parameters for (ansible.legacy.command) module: free_form. Supported parameters include: _raw_params, stdin_add_newline, argv, creates, warn, stdin, _uses_shell, executable, strip_empty_ends, chdir, removes."}
...ignoring
```

ドキュメントにも記載があるがargsの神髄は`create`、`chdir`、`remove`にある。
つまりコマンドを実行する場所について記載するときに使うのである。

## All yaml


```yaml
- hosts: localhost
  tasks:
    - name: Run builtin.command by one-line
      ansible.builtin.command: "echo hoge"

    - name: Run builtin.command with cmd
      ansible.builtin.command:
        cmd: "echo hoge"

    - name: Run builtin.command with argv as string.
      ansible.builtin.command:
        argv: "echo hoge"
      ignore_errors: true

    - name: Run builtin.command with argv as list.
      ansible.builtin.command:
        argv:
          - "echo"
          - "hoge"

    - name: Run builtin.command with args, cmd
      ansible.builtin.command:
      args:
        cmd: "echo hoge"
      ignore_errors: true

    - name: Run builtin.command with args, argv
      ansible.builtin.command:
      args:
        argv:
          - "echo"
          - "hoge"
      ignore_errors: true

    - name: Run builtin.command bad args
      ansible.builtin.command:
      args:
        free_form: "echo hoge"
      ignore_errors: true
```


