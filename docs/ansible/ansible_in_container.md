# Ansible in Container

## キーワード

- Ansible
 - docker

## ターゲット

- ansible を コンテナの中で実行しようとしている人
 - ansible-playbook内部でdockerを実行しようとしている人

## Version

- ansible-core 2.11, 2.12, 2.13

## 本文

ansibleコンテナには`[ansible-runner](https://quay.io/repository/ansible/ansible-runner)`や`[ansible-core](https://quay.io/repository/ansible/ansible-core)`があるが、これらは`init`で起動していない為、この内部でコンテナを実行する事ができない。
`podman`を使えばdaemonlessで動作する為、コンテナを扱う事はできるがpodmanはexec moduleが2021.12時点で存在しないため、プログラムをコンテナに押し込んだだけのコンテナ、というものを扱う上では利用できるとは言い難い。

`dind` image等を使うこともできるがdindのdocker imageは基本的にUbuntsuベースであることが多く、RedHat色が強まっているansibleを動かす上でも都合が悪い。
従ってこのための解法を考えてみる。

### DooD を試す

```bash
$ docker run -ti --rm -v /var/run/docker.sock:/var/run/docker.sock ansible-docker bash
# docker ps
```

どうやらまともに動く。
docker.socketにアクセスできるか否かはホスト設定にも依存するので注意が必要

## まとめ


## 参考リンク

- https://esakat.github.io/esakat-blog/posts/docker-in-docker/
- https://blog.nijohando.jp/post/docker-in-docker-docker-outside-of-docker/
- http://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/
- https://wand-ta.hatenablog.com/entry/2020/01/10/225755
- https://qiita.com/tomoten/items/01bcb58758b99455ac45