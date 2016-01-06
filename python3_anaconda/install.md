
# 動作環境

- MAC OS X Yosemite 10.10.5
- Virtualbox : 5.0.12 r104815
- Vagrant : Vagrant 1.7.4

### 0. VagrantでUbuntu14.04環境を作成する
所要時間目安:30分
```bash
vagrant box add ubuntu1404 https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box

mkdir ubuntu
cd ubuntu
vagrant init ubuntu1404
```

### 1. pyenvでAnacondaをインストールする

```bash
git clone https://github.com/yyuu/pyenv.git ~/.pyenv

echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
exec $SHELL

pyenv install --list | grep anaconda

pyenv install anaconda3-2.2.0

pyenv rehash

pyenv global anaconda3-2.2.0
```

### 2. Mecab をインストールする

```bash
sudo apt-get install libmecab-dev
sudo apt-get install mecab mecab-ipadic-utf8

mecab --version

pip install mecab-python3
python -c "import MeCab"
```

### 3. mecab-ipadic-neologd をインストールする

詳細は[neologd](https://github.com/neologd/mecab-ipadic-neologd/blob/master/README.ja.md)を参照。

```bash
sudo aptitude install mecab libmecab-dev mecab-ipadic-utf8 git make curl xz-utils

git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git
```

