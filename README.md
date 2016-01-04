# 概要

Ubuntu14.04にpyenv, Anaconda, MeCab をインストールした Python3の仮想環境を環境構築する。

# 確認した環境

## Virtualbox

バージョン 5.0.12 r104815

## Vagrant

Vagrant 1.7.4

## Box ファイル

http://www.vagrantbox.es/ より Ubuntu14.04のboxを追加しました。

```sh
$ vagrant box add ubuntu1404 https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box
```

# 構成

Ansible サーバー(node1)と、Ansible に制御される側の2台のサーバー(node2)を立てます。
`node2` にpython3環境を構築します。

`config.vm.provision` で一気に構築することもできます。

# 準備1

適当なディレクトリを作った後、下記のようなファイル構成まで準備します。
```sh

```

はじめに、

```sh
$ vagrant init ubuntu1404
```
Vagrantfileファイルを作成します。
このファイルの中身を下記に書き換えます。

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.define :node1 do |node|
    node.vm.box = "ubuntu1404"
    node.vm.network :forwarded_port, guest: 22, host: 2001, id: "ssh"
    node.vm.network :private_network, ip: "192.168.33.11"

    node.vm.provision :shell, path: "provision.sh"
  end

  config.vm.define :node2 do |node|
    node.vm.box = "ubuntu1404"
    node.vm.network :forwarded_port, guest: 22, host: 2002, id: "ssh"
    node.vm.network :forwarded_port, guest: 80, host: 8000, id: "http"
    node.vm.network :private_network, ip: "192.168.33.12"
  end

end
```

`provision.sh` は、
```sh
#!/bin/sh

sudo apt-get update
sudo apt-get upgrade
sudo apt-get -y install ansible
```
のような内容です。

