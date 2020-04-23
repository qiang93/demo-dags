ansible > v2.7.8
pip3 

sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

sudo apt install python3-pip -y
sudo pip3 install --upgrade pip
wget https://codeload.github.com/kubernetes-sigs/kubespray/zip/v2.12.5

Usage: https://github.com/kubernetes-sigs/kubespray#ansible

> ERROR: paramiko 2.7.1 has requirement cryptography>=2.5, but you'll have cryptography 2.1.4 which is incompatible.
paramiko信赖cryptography>=2.5
解决方法：
pip install cryptography==2.5


declare -a IPS=(47.57.126.35 47.57.121.116 47.57.127.20 47.57.131.26 47.57.126.78 47.57.127.209)
CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}


修改hosts.yaml ip为内网IP，并且开放PING


ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root --user=devops cluster.yml
