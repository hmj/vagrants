
# 0

```bash
vagrant box add ubuntu1404 https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box

mkdir ubuntu
cd ubuntu
vagrant init ubuntu1404
```

# 1

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
