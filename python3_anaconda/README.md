# 概要

Ubuntu14.04にpyenv, Anaconda, MeCab をインストールした Python3の仮想環境を環境構築する。

# 確認した環境

- Mac OS X
- Memory : 6GB 以上が望ましい

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
```bash
.
├── Vagrantfile
├── hosts
└── ssh_config
```

はじめに、

```bash
$ vagrant init ubuntu1404
```
Vagrantfileファイルを作成します。
このファイルの中身を下記に書き換えます。

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

$setup = <<SCRIPT
sudo apt-get update
sudo apt-get upgrade
sudo apt-get -y install git
sudo apt-get -y install ansible
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.define :node1 do |node|
    node.vm.box = "ubuntu1404"
    node.vm.network :forwarded_port, guest: 22, host: 2001, id: "ssh"
    node.vm.network :private_network, ip: "192.168.33.11"

    #node.vm.provision :shell, path: "provision.sh"
    node.vm.provision "shell", inline: $setup

    node.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "512"]
    end
  end

  config.vm.define :node2 do |node|
    node.vm.box = "ubuntu1404"
    node.vm.network :forwarded_port, guest: 22, host: 2002, id: "ssh"
    node.vm.network :forwarded_port, guest: 80, host: 8000, id: "http"
    node.vm.network :private_network, ip: "192.168.33.12"

    node.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
    end
  end

end
```

編集が終わったら
```sh
$ vagrant up
```
起動します。

Ansible で node1 から node2 へ ssh するための準備をします。
```sh
$ vagrant ssh-config node1 > ssh_config
$ scp -F ssh_config .vagrant/machines/node2/virtualbox/private_key node1:.ssh/id_rsa
```

# 準備2

node1 から ansible を実行する。

```sh
$ vagrant ssh node1

(node1) $ git clone https://gist.github.com/8e338a040b42685bd445.git playbook

(node1) $ ansible-playbook -i ../../vagrant/hosts playbook/mypython.yml --check

(node1) $ ansible-playbook -i ../../vagrant/hosts playbook/mypython.yml
```

