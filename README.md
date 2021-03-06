# ansible-playground

[Ansible](https://www.ansible.com/) playground to enjoy learning Ansible in a multi Operating System environment.

This playground use [Virtualbox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com) to provision virtual machines
to have where to play

This environment create five VMs (see [Vagrantfile](Vagrantfile)):

* One Centos 6 (hostname centos6)
* One Centos 7 (hostname centos7)
* One Centos 8 (hostname centos8) **--> default when execute vagrant up**
* One Ubuntu 14.04 (hostname ubuntu1404)
* One Ubuntu 16.04 (hostname ubuntu1604)
* One Ubuntu 18.04 (hostname ubuntu1804)
* One Ubuntu 18.10 (hostname ubuntu1810)
* One Ubuntu 19.04 (hostname ubuntu1904)
* One Debian 8 "Jessie" (hostname debian8)
* One Debian 9 "Stretch" (hostname debian9)
* One Debian 10 "Buster" (hostname debian10)
* One Amazon Linux 2 LTS (hostname amzn2)

All the five VMs could be used together at the same time or in any combination, each one of this has
two Network Interfaces:

* One in "nat" mode for internal Vagrant-VirtualBox communication (when you execute vagrant ssh "hostname")
* One in "bridge" mode to your favorite "Host Interface" (This is asked you when VM startup) and where [Ansible](https://www.ansible.com/) could work using ssh protocol

Of course, feel free to used it and adapted it to your personal project!, Let me know if you have
improvements using [Pull Request](https://github.com/christiangda/ansible-playground/pulls) or creating a [new Issue](https://github.com/christiangda/ansible-playground/issues)

## My environment

I'm using Linux [Fedora 31 Workstation](https://getfedora.org/workstation) with the latest updates

## Install Requirements

Requirements to start play are:

* [git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com)
* [Python3](https://www.python.org)
* [Ansible](https://www.ansible.com/)

### Install git

```bash
sudo dnf install git
```

### Install Visual Studio Code

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'

dnf check-update
sudo dnf install code
```

Install vscode plugins

```bash
code --install-extension vscoss.vscode-ansible
code --install-extension streetsidesoftware.code-spell-checker
code --install-extension yzhang.markdown-all-in-one
code --install-extension bbenoist.vagrant
code --install-extension marcostazi.vs-code-vagrantfile
code --install-extension ms-python.python
```

### Install VirtualBox

I recommend the following link for this work [Virtualbox - fedora](https://www.if-not-true-then-false.com/2010/install-virtualbox-with-yum-on-fedora-centos-red-hat-rhel/)

```bash
sudo wget http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo -O /etc/yum.repos.d/virtualbox.repo
sudo dnf install -y binutils gcc make patch libgomp glibc-headers glibc-devel kernel-headers kernel-devel dkms qt5-qtx11extras libxkbcommon
sudo dnf update
sudo dnf install VirtualBox-6.0
sudo /usr/lib/virtualbox/vboxdrv.sh setup
sudo usermod -a -G vboxusers $USER
```

### Install Vagrant

check always you are using the last version

```bash
sudo dnf install -y https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.rpm
```

#### Vagrant plugins

Install [vagrant-hostmanager](https://github.com/devopsgroup-io/vagrant-hostmanager)

```bash
vagrant plugin install vagrant-hostmanager
```

## Start playing

First of all clone this repository from [https://github.com/christiangda/ansible-playground](https://github.com/christiangda/ansible-playground)

```bash
cd ~
mkdir -p ansible-workspace
cd ansible-workspace/
git clone https://github.com/christiangda/ansible-playground.git
cd ansible-playground/
```

The better way to **" isolate the blast radius " -> ;)** is using [Python Virtual Environments](https://docs.python.org/3/library/venv.html), so create it

```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install pylint
pip install rope
pip install autopep8
pip install ansible
```

As I mentioned before you have five VMs to play for (centos6, centos7, ubuntu1804, ubuntu1810, amzn2, etc), so
you can startup altogether or only one, two or three if you prefer

If you want to startup only one

```bash
vagrant up ubuntu1804
```

If you want to play with two, one Ubuntu, one Centos

```bash
vagrant up ubuntu1810 centos7 debian9 debian10
```

To startup the five together, one by one (it take a long time ;), go for a coffee cup) and when you go back
your console maybe is asking for the network interface you want to use as a
bridge for ansible machines

```bash
vagrant up centos6 centos7 centos8 ubuntu1804 ubuntu1810 amzn2
```

Test if ansible is working fine

```bash
ansible all -m ping -o
```

get hostname of every server

```bash
ansible all -m command -a 'hostname -s' -o
```

More ad-hoc commands

```bash
ansible all -m setup -a 'filter=ansible_distribution' -o
ansible all -m setup -a 'filter=ansible_os_family' -o
ansible all -m setup -a 'filter=ansible_distribution_version' -o
ansible all -m setup -a 'filter=ansible_distribution_major_version' -o
```

Testing the first-playbook.yaml

```bash
ansible-playbook first-playbook.yaml
```

if you want to use filter

```bash
ansible-playbook first-playbook.yaml --limit centos7,ubuntu1804
```

Connect to your servers:

```bash
vagrant ssh [centos6 | centos7]
# or
vagrant ssh [ubuntu1404 | ubuntu1604 | ubuntu1804 | ubuntu1810]
# or
vagrant ssh [debian8 | debian9]
# or
vagrant ssh amzn2
```

If you want to know what servers you startup

```bash
vagrant status
```

## Stop Playing

Deactivate virtual env

```bash
deactivate
```

Stop (in vagrant halt) vagrant Virtual machines

```bash
vagrant halt
```

If you want to stop and destroy vagrant Virtual machines

```bash
vagrant destroy -f
```

## License

This repository code is released under the GNU General Public License Version 3:

* [http://www.gnu.org/licenses/gpl-3.0-standalone.html](http://www.gnu.org/licenses/gpl-3.0-standalone.html)

## Author Information

* [Christian Gonz??lez](https://github.com/christiangda)
