# 使用 Ansible 自动安装 HA RKE2 Kubernetes 集群

> 原文：<https://blog.devgenius.io/automating-installation-of-ha-rke2-kubernetes-cluster-with-ansible-2456cc95d970?source=collection_archive---------1----------------------->

## 使用 kube-vip 创建 Ansible 清单并安装高可用性(HA) RKE2 集群。

![](img/3373d1abb973d0f5a6610cc52795d722.png)

虽然有不同的方法来安装 Kubernetes 集群，尤其是在云平台中的托管集群，但仍然有一些情况下我们需要在 DC 中的裸机上安装它。因此，根据您的需求，可能会出现一些与安装相关的问题，包括如何自动化该过程，使其灵活且可重新部署？

我们将使用 [RKE2](https://docs.rke2.io) 来供应和管理 Kubernetes 集群， [kube-vip](https://kube-vip.chipzoller.dev/) 用于 HA 故障转移解决方案，而 [Ansible](https://www.ansible.com) 用于自动化安装过程，使其具有灵活性和可重新部署性。

在开始之前，我想简单地提示一下我们要用 Ansible 创建和应用的步骤和文件的顺序，以便用 kube-vip 准备好我们的 HA RKE2 集群。

```
.
├── **inventory_file** # Hostnames of master and agent nodes (**1**)
├── **env_vars.yaml** # The environment variables (**2**)
├── **rke2-install-server.yml** # The first master node (**3**)
├── **kube-vip.yaml** # For kube-vip installation (**4**)
└── **token.enc** # Encrypted TOKEN (**5**)
├── **rke2-add-server.yaml** # For additional master nodes (**6**)
├── **rke2-add-agent-node.yaml** # For adding the agent nodes (**7**)

0 directories, 7 files
```

为了拥有一个 HA [RKE2](https://docs.rke2.io/) 集群，我们需要:

*   至少 3 个主节点
*   控制平面节点之间具有浮动 IP 地址的故障切换解决方案
    ***** DNS 记录指向该浮动 IP 地址
    *****[**kube-VIP**](https://kube-vip.chipzoller.dev/docs)**。**对于高可用性故障转移解决方案，我将选择 [**kube-vip**](https://kube-vip.chipzoller.dev/docs) ，与其他类似工具相比，它本身支持控制平面健康监视(*控制平面负载平衡*)以及许多其他有用的[功能](https://kube-vip.chipzoller.dev/docs/about/features)
*   3 个工作节点，用于集群中的插件和其他工作负载。

> 让我们继续讨论 [*Ansible*](https://www.ansible.com) *清单。*

在我们的例子中，ansi ble**$ Inventory _ File**(用您的库存文件替换它)的内容是

*   **。/inventory_file**

```
all:
  hosts:
    example-host1: # master node
    example-host2: # master node
    example-host3: # master node
    example-host4: # agent node
    example-host5: # agent node
    example-host6: # agent node
```

为了避免重用这些值，让我们创建环境变量文件，供以后在 ansible 清单中使用:

*   **。/env_vars.yaml**

```
##################################################
##################################################
###               RKE2 Configs                 ###
##################################################
##################################################

WATCHDIR: "/var/lib/rancher/rke2/server/manifests"
CONFDIR: "/etc/rancher/rke2"
CONFFILE: "{{ CONFDIR }}/config.yaml"
SERVER_FAILOVER_IP: "w.x.y.z" **# Replace with your failover floating IP address.**
SERVER_ADDRESS_SHORT: "example-hostname" **# Hostname pointed to failover floating IP (**w.x.y.z**), change with yours.**
SERVER_ADDRESS_FQDN: "example-hostname.somedomain.tld" **# FQDN pointed to failover floating IP (**w.x.y.z**), change with yours.**

##################################################
##################################################
###               RKE2 Version                 ###
##################################################
##################################################

INSTALL_RKE2_VERSION: "v1.22.4+rke2r2" **# Change with your preferred version.**

##################################################
##################################################
###                kube-vip                    ###
##################################################
##################################################

INTERFACE: "ens192" **# Change with yor IFACE**
VIP: "{{ SERVER_FAILOVER_IP }}"
CONTAINER_RUNTIME_ENDPOINT: "unix:///run/k3s/containerd/containerd.sock"
CONTAINERD_ADDRESS: "/run/k3s/containerd/containerd.sock"
KUBE_VIP_IMAGE: "docker.io/plndr/kube-vip"
KUBE_VIP_IMAGE_TAG: "latest"
```

开始时，我们需要安装第一个主节点。让我们为此创建一个 ansible 清单。

*   **。/rke2-install-server.yaml**

```
- name: Install rke2 first server instance
  hosts: example-host1
  become: true
  vars_files:
    - env_vars.yaml 

  tasks:
  - name: create directories if they don't exist
    file:
      path: "{{ item }}"
      state: directory
      owner: root
      group: root
      mode: 0755
      recurse: yes
    loop:
      - "{{ CONFDIR }}"
      - "{{ WATCHDIR }}"

  - name: get hostname short
    shell: |
      hostname -s
    register: hostname_short
  - name: hostname fqdn
    shell: |
      hostname -f
    register: hostname_fqdn

  - name: Print hostname fqdn and short
    debug:
      msg:
      - "hostname short '{{ hostname_short.stdout }}'"
      - "hostname fqdn: '{{ hostname_fqdn.stdout }}'"

  - name: Creating the config.yaml
    copy:
      dest: "{{ CONFFILE }}"
      content: |
        tls-san:
          - "{{ hostname_short.stdout }}"
          - "{{ hostname_fqdn.stdout }}"
          - "{{ SERVER_ADDRESS_SHORT }}"
          - "{{ SERVER_ADDRESS_FQDN }}"
          - "{{ SERVER_FAILOVER_IP }}"

        cni: "canal" # optional
    notify: "rke2-server service restart"

  - name: Creating the CoreDNS config file
    copy:
      dest: "{{ WATCHDIR }}/rke2-coredns-config.yaml"
      content: |
        apiVersion: helm.cattle.io/v1
        kind: HelmChartConfig
        metadata:
          creationTimestamp: null
          name: rke2-coredns
          namespace: kube-system
        spec:
          valuesContent: |-
            nodelocal:
              enabled: true **# Change this to false or remove this (*Creating the CoreDNS config file*) section altogether if you don’t want to install *NodeLocal DNSCache***
          bootstrap: true

  - name: Installing rke2 server
    shell: |
      curl -sfL https://get.rke2.io | INSTALL_RKE2_CHANNEL=stable INSTALL_RKE2_VERSION="{{ INSTALL_RKE2_VERSION }}" sh -
    notify: "rke2-server service start"

  handlers:
    - name: Make sure a service unit is running
      systemd:
        state: started
        name: rke2-server
      listen: "rke2-server service start"

    - name: Make sure a service unit is restarted
      systemd:
        state: restarted
        name: rke2-server
      listen: "rke2-server service restart"
```

创建第一个主节点的 ansible 命令。

```
**ansible-playbook -u $USERNAME -i $Inventory_File -l example-host1 ./rke2-install-server.yaml**
```

第一个主节点准备好了。

您可以在***/etc/rancher/rke 2/rke 2 . YAML***文件中获取 kubectl 配置，默认情况下，服务器地址是 127.0.0.1，用您设置的主机名替换 example-host1，以便远程访问。稍后，在 kube-vip 安装后，我们将使用我们的 HA 浮动 ip 地址或主机名对其进行更改。

在安装第二个和第三个主节点和其他代理节点之前，应该安装 [kube-vip](https://kube-vip.chipzoller.dev/docs) 。

*让我们为****kube-VIP****安装*创建一个 ansible 清单文件

*   **。/kube-vip.yaml**

```
- name: Install kube-vip
  hosts: example-host1
  become: true
  vars_files:
    - env_vars.yaml  

  tasks:
  - name: Download kube-vip RBAC file
    get_url:
      url: https://kube-vip.io/manifests/rbac.yaml
      dest: "{{ WATCHDIR }}/kube-vip-rbac.yaml"
      owner: root
      group: root
      mode: 0644

  - name: Deploying kube-vip
    shell: |
      ctr --namespace k8s.io image pull "{{ KUBE_VIP_IMAGE }}":"{{ KUBE_VIP_IMAGE_TAG }}"
      ctr --namespace k8s.io run --rm --net-host "{{ KUBE_VIP_IMAGE }}":"{{ KUBE_VIP_IMAGE_TAG }}" vip /kube-vip \
      manifest daemonset \
      --arp \
      --interface "{{ INTERFACE }}" \
      --address "{{ VIP }}" \
      --controlplane \
      --leaderElection \
      --taint \
      --services \
      --inCluster | tee "{{ WATCHDIR }}/kube-vip.yaml"
```

**安装 kube-vip 的 ansible 命令:**

```
**ansible-playbook -u $USERNAME -i $Inventory_File -l example-host1 ./kube-vip.yaml**
```

在此步骤之后，浮动 IP 地址由 kube-vip daemonset 在第一个主节点示例 host1 上提出，因为 *RKE2 监视****/var/lib/rancher/rke 2/server/manifests***(在我们的 ansible variables 文件中，将定义为 **WATCHDIR** )目录中的所有文件，并将其安装到集群中，因此不需要在

要查看 kube-vip pod 和 ip 是否正在运行/就绪，您可以使用以下 [**kubectl**](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) 命令:

```
**kubectl -n kube-system logs $(kubectl -n kube-system get po |grep kube-vip-ds|awk '{print $1}')**
```

我们将使用这个浮动 IP 地址或主机名*示例-hostname.somedomain.tld* 指向该 IP 地址作为我们在***/etc/rancher/rke 2/config . YAML***文件中的服务器地址，用于将其他主节点或代理节点加入到集群中。

现在是时候将额外的主节点和代理节点加入集群了。
为了加入它们，我们需要在*示例-host1* 服务器中的***/var/lib/rancher/rke 2/server/node-token***文件中获取令牌，以便稍后将其放入带有 ansible 的附加加入节点的***/etc/rancher/rke 2/config . YAML***文件中。
由于**令牌**文件的内容很敏感，我们需要在 ansible 目录和清单中加密保存和使用它。下面是在我们的 ansible 清单中将它作为环境变量安全保存和使用的步骤。

**加密令牌:**

```
**ssh example-host1 'cat /var/lib/rancher/rke2/server/node-token'**Copy the output.**cat > ./token.enc << \EOF**
TOKEN: *PASTE_HERE_THE_TOKEN::STRING_IN_THE:node-token_FILE*
**EOF****ansible-vault encrypt ./token.enc**New Vault password:
Confirm New Vault password:
Encryption successful
```

现在，在我们加密了令牌之后，让我们创建一个 ansible 清单，用于添加两个额外的主节点示例-host2 和示例-host3:

*   **。/rke2-add-server.yaml**

```
- name: install rke2 server
  hosts: ~example-host[2-3]
  become: true
  vars_files:
    - env_vars.yaml
    - token.enc  # *The encrypted TOKEN file, later it is decrypted in the command-line with the* ***--ask-vault-pass*** *option.*

  tasks:
  - name: create directories if they don't exist
    file:
      path: "{{ item }}"
      state: directory
      owner: root
      group: root
      mode: 0755
      recurse: yes
    loop:
      - "{{ CONFDIR }}"
      - "{{ WATCHDIR }}"

  - name: get hostname short
    shell: |
      hostname -s
    register: hostname_short
  - name: hostname fqdn
    shell: |
      hostname -f
    register: hostname_fqdn

  - name: Print hostname fqdn and short
    debug:
      msg:
      - "hostname short '{{ hostname_short.stdout }}'"
      - "hostname fqdn: '{{ hostname_fqdn.stdout }}'"

  - name: Creating the config.yaml
    copy:
      dest: "{{ CONFFILE }}"
      content: |
        token: {{ TOKEN }}
        server: https://{{ SERVER_ADDRESS_FQDN }}:9345
        tls-san:
          - "{{ hostname_short.stdout }}"
          - "{{ hostname_fqdn.stdout }}"
          - "{{ SERVER_ADDRESS_SHORT }}"
          - "{{ SERVER_ADDRESS_FQDN }}"
          - "{{ SERVER_FAILOVER_IP }}"
    notify: "rke2-server service restart"

  - name: Installing rke2 server
    shell: |
      curl -sfL https://get.rke2.io | INSTALL_RKE2_CHANNEL=stable INSTALL_RKE2_VERSION="{{ INSTALL_RKE2_VERSION }}" sh -
    notify: "rke2-server service start"

  handlers:
  - name: Make sure a service unit is running
    systemd:
      state: started
      name: rke2-server
    listen: "rke2-server service start"

  - name: Make sure a service unit is restarted
    ansible.builtin.systemd:
      state: restarted
      name: rke2-server
    listen: "rke2-server service restart"
```

**请注意**，在下面的命令中，主节点被一个接一个地添加，就好像你同时启动两个主节点，第二个主节点可能会失败。这是因为集群中有太多的学习者 etcd 成员。
在添加代理节点的情况下，我们可以毫无问题地同时添加所有代理节点。

**可行命令:**

```
**ansible-playbook -u $USERNAME -i $Inventor_File ./rke2-add-server.yaml -l example-host2 --ask-vault-pass**Vault password:**ansible-playbook -u $USERNAME -i $Inventor_File ./rke2-add-server.yaml -l example-host3 --ask-vault-pass**Vault password:
```

对于保险库密码:在加密****token . enc***文件的内容时，输入您在上面设置的相同密码。*

*最后，让我们将 3 个代理节点添加到群集，正如我在上面提到的，我们可以同时添加所有这些节点。*

*   ***。/rke2-add-agent-node.yaml***

```
*- name: install rke2 agent node
  hosts: all:!~example-host[1-3] # Including all hosts and excluding the master nodes.
  become: true
  vars_files:
    - env_vars.yaml
    - token.enc  # *The encrypted TOKEN file, later it is decrypted in the command-line with the* ***--ask-vault-pass*** *option.*

  tasks:
  - name: create directories if they don't exist
    file:
      path: "{{ item }}"
      state: directory
      owner: root
      group: root
      mode: 0755
      recurse: yes
    loop:
      - "{{ CONFDIR }}"

  - name: get hostname short
    shell: |
      hostname -s
    register: hostname_short
  - name: hostname fqdn
    shell: |
      hostname -f
    register: hostname_fqdn

  - name: Print hostname fqdn and short
    debug:
      msg:
      - "hostname short '{{ hostname_short.stdout }}'"
      - "hostname fqdn: '{{ hostname_fqdn.stdout }}'"

  - name: Creating the config.yaml
    copy:
      dest: "{{ CONFFILE }}"
      content: |
        token: {{ TOKEN }}
        server: https://{{ SERVER_ADDRESS_FQDN }}:9345

  - name: Installing rke2 agent
    shell: |
      curl -sfL https://get.rke2.io | INSTALL_RKE2_CHANNEL=stable INSTALL_RKE2_VERSION="{{ INSTALL_RKE2_VERSION }}" sh -
    notify: "rke2-agent service start"

  handlers:
  - name: Make sure a service unit is running
    systemd:
      state: started
      name: rke2-agent
    listen: "rke2-agent service start"

  - name: Make sure a service unit is restarted
    ansible.builtin.systemd:
      state: restarted
      name: rke2-agent
    listen: "rke2-agent service restart"*
```

***接下来，键入命令:***

```
***ansible-playbook -u $USERNAME -i $Inventor_File ./rke2-add-agent-node.yaml --ask-vault-pass**Vault password:*
```

*我们的 HA RKE2 群集准备好了 3 个主节点和 3 个代理节点，通过 kube-vip 以及启用的 NodeLocal DNSCache 在控制计划节点之间提供故障转移浮动 IP。*