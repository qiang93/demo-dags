# 执行机需满足如下条件
ansible > v2.7.8
pip3

> 建立免密钥通信后所有操作可通过ansible来管理，

# 安装ansible
``` 
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```
# 安装pip3
```
sudo apt install python3-pip -y
sudo pip3 install --upgrade pip
```
# 安装release版本
```
wget https://codeload.github.com/kubernetes-sigs/kubespray/zip/v2.12.5
unzip v2.12.5
cd kubespray-2.12.5
```
# Usage
* Install dependencies from ``requirements.txt`` 

  sudo pip3 install -r requirements.txt

  > ERROR: paramiko 2.7.1 has requirement cryptography>=2.5, but you'll have cryptography 2.1.4 which is incompatible.
    ```
    paramiko信赖cryptography>=2.5
    解决方法：
    pip install cryptography==2.5
    ```
* Copy ``inventory/sample`` as ``inventory/mycluster`` 

  cp -rfp inventory/sample inventory/mycluster

* Update Ansible inventory file with inventory builder 

  declare -a IPS=(10.10.1.3 10.10.1.4 10.10.1.5)
  CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}

* Review and change parameters under ``inventory/mycluster/group_vars`` 

  cat inventory/mycluster/group_vars/all/all.yml
  cat inventory/mycluster/group_vars/k8s-cluster/k8s-cluster.yml

* exec ansible-playbooks
  > Deploy Kubespray with Ansible Playbook - run the playbook as root
  > The option `--become` is required, as for example writing SSL keys in /etc/,
  > installing packages and interacting with various systemd daemons.
  > Without --become the playbook will fail to run!
  ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root --user=devops cluster.yml

* 注意事项
  > ansible_host：是ansible远程执行的地址，ip、access_ip是服务地址，在公有云服务器上要注意
  > 需要关闭防火墙，服务之间需要能ping通信
  
# 添加节点
  1、建立免密钥认证，hosts.yaml添加相应的host
  2、--limit=node1限制Kubespray来避免干扰群集中的其他节点
  
    ```
    ansible-playbook -i inventory/mycluster/hosts.yaml scale.yml --limit=node5
    ```
# 删除节点
  1、驱除节点pod
  
    ```
    kubectl drain NODE_NAME
    ```
  2、传递-e node=NODE_NAME参数，以将执行限制为要删除的节点。
  
    ```
    ansible-playbook -i inventory/mycluster/hosts.yaml remove-node.yml -e node=node4
    ```
# 升级集群
  
