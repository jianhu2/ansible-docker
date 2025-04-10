# 本地软件准备

支持linux/macOS上运行。

## 环境依赖：

依赖 python3-pip，

* [安装ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
  ，以ubuntu2204为例

```

sudo apt update
# 更新python版本
sudo apt install python3-pip -y

# 安装ansible
sudo apt install ansible

# 验证 pip 是否安装成功：
python3 -m pip --version
pip 22.0.2 from /usr/lib/python3/dist-packages/pip (python 3.10)

```

# 快速安装docker

1.在hosts.yml中添加要部署的ssh登录信息/服务器内外网IP地址，比如:

```yaml
dev: #这个命名在1中被引用
  hosts:
    192.168.50.60:
  vars:
    ansible_user: root
    ansible_port: 22
    ansible_ssh_private_key_file: ~/.ssh/id_ed25519
    INTRANET_IP: "192.168.50.60"
    INTERNET_IP: "192.168.50.60"
```   

2.运行ansible，会同时安装docker/docker-compose等:

```ansible-playbook -i ./hosts.yml ./install_docker.yml```

```shell
# ansible-playbook -i ./hosts.yml ./install_docker.yml

PLAY [dev] ***************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************************************************************
[WARNING]: Platform linux on host 192.168.50.130 is using the discovered Python interpreter at /usr/bin/python3.12, but future installation of another Python interpreter could change the meaning of that
path. See https://docs.ansible.com/ansible-core/2.17/reference_appendices/interpreter_discovery.html for more information.
ok: [192.168.50.130]

TASK [init_system : include_tasks] ***************************************************************************************************************************************************************************
skipping: [192.168.50.130]

TASK [init_system : include_tasks] ***************************************************************************************************************************************************************************
skipping: [192.168.50.130]

TASK [init_system : include_tasks] ***************************************************************************************************************************************************************************
included: /root/moss-test/ansible-docker/roles/init_system/tasks/docker.yml for 192.168.50.130

TASK [init_system : include_tasks] ***************************************************************************************************************************************************************************
skipping: [192.168.50.130]

TASK [init_system : include_tasks] ***************************************************************************************************************************************************************************
included: /root/moss-test/ansible-docker/roles/init_system/tasks/docker_ubuntu.yml for 192.168.50.130

TASK [init_system : 检查 SELinux 配置文件是否存在] ***********************************************************************************************************************************************************
ok: [192.168.50.130]

TASK [init_system : 配置文件禁用SELinux] *********************************************************************************************************************************************************************
skipping: [192.168.50.130]

TASK [init_system : 立即禁用SELinux] *************************************************************************************************************************************************************************
skipping: [192.168.50.130]

TASK [init_system : 安装一些依赖包] **************************************************************************************************************************************************************************
ok: [192.168.50.130] => (item=apt-transport-https)
ok: [192.168.50.130] => (item=ca-certificates)
ok: [192.168.50.130] => (item=curl)
ok: [192.168.50.130] => (item=gnupg-agent)
ok: [192.168.50.130] => (item=software-properties-common)

TASK [init_system : 添加Docker GPG key] **********************************************************************************************************************************************************************
ok: [192.168.50.130]

TASK [init_system : 添加Docker APT仓库] **********************************************************************************************************************************************************************
changed: [192.168.50.130]

TASK [init_system : 安装docker及containerd] ******************************************************************************************************************************************************************

changed: [192.168.50.130] => (item=containerd.io)

changed: [192.168.50.130] => (item=docker-ce-cli)

changed: [192.168.50.130] => (item=docker-ce)

TASK [init_system : 二进制安装docker-compose] ****************************************************************************************************************************************************************
changed: [192.168.50.130]

TASK [init_system : 验证docker版本] **************************************************************************************************************************************************************************
ok: [192.168.50.130]

TASK [init_system : 验证docker-compose版本] ******************************************************************************************************************************************************************
ok: [192.168.50.130]

TASK [init_system : Docker version] **************************************************************************************************************************************************************************
ok: [192.168.50.130] => {
    "docker_version_output.stdout": "Docker version 28.0.1, build 068a01e"
}

TASK [init_system : Docker-Compose version] ******************************************************************************************************************************************************************
ok: [192.168.50.130] => {
    "docker_compose_version_output.stdout": "Docker Compose version v2.33.1"
}

TASK [init_system : 创建目录/etc/docker/] ********************************************************************************************************************************************************************
ok: [192.168.50.130]

TASK [init_system : 拷贝docker配置daemon.json] ***************************************************************************************************************************************************************
ok: [192.168.50.130]

TASK [init_system : 取消journald作为Docker日志引擎] **********************************************************************************************************************************************************
ok: [192.168.50.130]

TASK [init_system : 启动Docker守护进程并让它关机重启也生效] **************************************************************************************************************************************************
changed: [192.168.50.130]

PLAY RECAP ***************************************************************************************************************************************************************************************************
192.168.50.130             : ok=17   changed=1    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0
```
