---
title: "Ansible modules utils.update_fact"
date:
draft: false
---
# ansible.utils.update_fact

- https://docs.ansible.com/ansible/latest/collections/ansible/utils/update_fact_module.html#ansible-collections-ansible-utils-update-fact-module

## About this page.

`set_fact`で定義した変数を文字通り更新するためのモジュール。
ただし、例に記載されている通り実際には`set_fact`値を更新するのでは無い為、
元々の変数を更新したい場合は再度、`set_fact`で上書きしてやらねばならない。

どういうことかというと以下の通り。


```yaml
---
- hosts: localhost
  gather_facts: false
  tasks:
    - name: Define set_fact
      ansible.builtin.set_fact:
        example_fact: {
          'Name': 'Sarav',
          'Email': 'aksarav.middlewareinventory.com',
          'location': 'chennai',
          'opt_dict': {
            'key' : "key1",
            'value': "value1"
          }
        }

    - name: "Updated facts."
      ansible.utils.update_fact:
        updates:
          - path: example_fact.opt_dict
            value: { 'key': 'key2', 'value': "value2" }
      register: _updated

    - name: "Show example_fact"
      ansible.builtin.debug:
        msg: "{{ example_fact }}"

    - name: "Show updated_fact"
      ansible.builtin.debug:
        msg: "{{ _updated }}"

    - name: "Update set_fact."
      ansible.builtin.set_fact:
        example_fact: "{{ _updated }}"

    - name: "Show example_fact again."
      ansible.builtin.debug:
        msg: "{{ example_fact }}"



```

```bash
$ ansible-playbook -i hosts sandbox-playbook.yml

PLAY [localhost] *****************************************************************************************

TASK [Define set_fact] ***********************************************************************************
ok: [localhost]

TASK [Updated facts.] ************************************************************************************
changed: [localhost]

TASK [Show example_fact] *********************************************************************************
ok: [localhost] => {
    "msg": {
        "Email": "aksarav.middlewareinventory.com",
        "Name": "Sarav",
        "location": "chennai",
        "opt_dict": {
            "key": "key1",
            "value": "value1"
        }
    }
}

TASK [Show updated_fact] *********************************************************************************
ok: [localhost] => {
    "msg": {
        "changed": true,
        "example_fact": {
            "Email": "aksarav.middlewareinventory.com",
            "Name": "Sarav",
            "location": "chennai",
            "opt_dict": {
                "key": "key2",
                "value": "value2"
            }
        },
        "failed": false
    }
}

TASK [Update set_fact.] **********************************************************************************
ok: [localhost]

TASK [Show example_fact again.] **************************************************************************
ok: [localhost] => {
    "msg": {
        "changed": true,
        "example_fact": {
            "Email": "aksarav.middlewareinventory.com",
            "Name": "Sarav",
            "location": "chennai",
            "opt_dict": {
                "key": "key2",
                "value": "value2"
            }
        },
        "failed": false
    }
}

PLAY RECAP ***********************************************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

上記の通り、単に`update_fact`しただけでは元の変数に変化が無いことが分かる。
