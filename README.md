kubeadm
=========

`kubeadm` is an ansible role to deploy kubernetes cluster based on kubeadm.
to learn more about kubeadm, follow the official doc: [kubeadm docs](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/).


**NOTE**: **Be sure you have read at least the `Requirements`and `Dependencies`README's sections before using the role**.

Requirements
------------
The module `community.general.modprobe` is called in this role.
in order to make this works properly, you have to install the `community.general`collection of ansible. the command to achieve this is right below.

```shell
$ ansible-galaxy collection install community.general
```

Role Variables
--------------
variables for compatible OS distros (`vars/main.yml`).
```yaml
kubeadm_compatible_os_distros:
  - debian
  - redhat
```

variables for minimum hardawares requirements (`vars/main.yml`)

```yaml
kubeadm_hardwares_min_requirements:
  ram: 2
  cpu: 2
```

systems packages variables (`defaults/main.yml`).
```yaml
kubeadm_system_package:
  - apt-transport-https
  - ca-certificates
  - curl
```

kubernetes packages and versions (`defaults/main.yml`).
```yaml
kubeadm_packages:
  - kubeadm=1.27.3-00
  - kubelet=1.27.3-00
  - kubectl=1.27.3-00
```

kubernetes networks modules (`defaults/main.yml`).
```yaml
kubeadm_network_modules:
  - overlay
  - br_netfilter
```

kubernetes networks parameters (`defaults/main.yml`).
```yaml
kubeadm_network_parameters:
  - key: net.bridge.bridge-nf-call-iptables
    value: 1
  - key: net.bridge.bridge-nf-call-ip6tables
    value: 1
  - key: net.ipv4.ip_forward
    value: 1
```

kubadm control plane node options (`defaults/main.yml`).
```yaml
kubeadm_init_config:
  apiserver_advertise_address: ''
  apiserver_bind_port: 6443
  apiserver_cert_extra_sans: ''
  cert_dir: /etc/kubernetes/pki
  certificate_key: ''
  control_plane_endpoint: ''
  cri_socket: ''
  dry_run: false
  feature_gates: ''
  ignore_preflight_errors: ''
  image_repositoriy: registry.k8s.io
  kubernetes_version: stable-1
  node_name: ''
  pod_network_cidr: 192.168.0.0/16
  service_cidr: 10.96.0.0/12
  service_dns_domain: cluster.local
  skip_certificate_key: true
  skip_phases: []
  skip_token_print: true
  token: ''
  token_ttl: ''
  upload_certs: true
```

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------
As say on top, this role is focus on deploying a kubernetes cluster based on kubeadm tool.

The role `kubeadm`does not install any **`container runtime`** on machines.
you have have to find a way to install a container runtime (containerd, docker ...)

in our case, we have used the public role [geerlingguy/ansible-role-containerd](https://github.com/geerlingguy/ansible-role-containerd) to install a containerd as container runtime.

```yaml
- name: geerlinguy.containerd
  version: 1.3.1
  src: https://github.com/geerlingguy/ansible-role-containerd.git
  scm: git
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```yaml
- name: Deploy kubernetes cluster
  hosts: all
  roles:
    - kubeadm
```
License
-------

BSD

Author Information
------------------

+ Jean Emmanuel ASSAMOI **`DevSecOps engineer`**
