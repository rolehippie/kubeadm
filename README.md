# kubeadm

[![Source Code](https://img.shields.io/badge/github-source%20code-blue?logo=github&logoColor=white)](https://github.com/rolehippie/kubeadm) [![Testing Build](https://github.com/rolehippie/kubeadm/workflows/testing/badge.svg)](https://github.com/rolehippie/kubeadm/actions?query=workflow%3Atesting) [![Readme Build](https://github.com/rolehippie/kubeadm/workflows/readme/badge.svg)](https://github.com/rolehippie/kubeadm/actions?query=workflow%3Areadme) [![Galaxy Build](https://github.com/rolehippie/kubeadm/workflows/galaxy/badge.svg)](https://github.com/rolehippie/kubeadm/actions?query=workflow%3Agalaxy) [![License: Apache-2.0](https://img.shields.io/github/license/rolehippie/kubeadm)](https://github.com/rolehippie/kubeadm/blob/master/LICENSE) 

Ansible role to install kubeadm and bootstrap Kubernetes. 

## Sponsor 

[![Proact Deutschland GmbH](https://proact.eu/wp-content/uploads/2020/03/proact-logo.png)](https://proact.eu) 

Building and improving this Ansible role have been sponsored by my employer **Proact Deutschland GmbH**.

## Table of content

* [Default Variables](#default-variables)
  * [kubeadm_apiserver_certsans](#kubeadm_apiserver_certsans)
  * [kubeadm_apiserver_endpoint](#kubeadm_apiserver_endpoint)
  * [kubeadm_bootstrap_expire](#kubeadm_bootstrap_expire)
  * [kubeadm_bootstrap_token](#kubeadm_bootstrap_token)
  * [kubeadm_calico_manifests](#kubeadm_calico_manifests)
  * [kubeadm_calico_version](#kubeadm_calico_version)
  * [kubeadm_canal_manifests](#kubeadm_canal_manifests)
  * [kubeadm_canal_version](#kubeadm_canal_version)
  * [kubeadm_cloud_provider](#kubeadm_cloud_provider)
  * [kubeadm_cluster_configuration](#kubeadm_cluster_configuration)
  * [kubeadm_cluster_name](#kubeadm_cluster_name)
  * [kubeadm_cri_socket](#kubeadm_cri_socket)
  * [kubeadm_default_apiserver_args](#kubeadm_default_apiserver_args)
  * [kubeadm_default_controller_args](#kubeadm_default_controller_args)
  * [kubeadm_default_kubelet_args](#kubeadm_default_kubelet_args)
  * [kubeadm_default_scheduler_args](#kubeadm_default_scheduler_args)
  * [kubeadm_extra_apiserver_args](#kubeadm_extra_apiserver_args)
  * [kubeadm_extra_controller_args](#kubeadm_extra_controller_args)
  * [kubeadm_extra_kubelet_args](#kubeadm_extra_kubelet_args)
  * [kubeadm_extra_scheduler_args](#kubeadm_extra_scheduler_args)
  * [kubeadm_flannel_manifests](#kubeadm_flannel_manifests)
  * [kubeadm_flannel_version](#kubeadm_flannel_version)
  * [kubeadm_general_networking](#kubeadm_general_networking)
  * [kubeadm_init_configuration](#kubeadm_init_configuration)
  * [kubeadm_join_configuration](#kubeadm_join_configuration)
  * [kubeadm_kubelet_configuration](#kubeadm_kubelet_configuration)
  * [kubeadm_kubeproxy_configuration](#kubeadm_kubeproxy_configuration)
  * [kubeadm_kubernetes_version](#kubeadm_kubernetes_version)
  * [kubeadm_kuberouter_manifests](#kubeadm_kuberouter_manifests)
  * [kubeadm_kuberouter_version](#kubeadm_kuberouter_version)
  * [kubeadm_local_address](#kubeadm_local_address)
  * [kubeadm_local_port](#kubeadm_local_port)
  * [kubeadm_master_nodes](#kubeadm_master_nodes)
  * [kubeadm_network_provider](#kubeadm_network_provider)
  * [kubeadm_worker_nodes](#kubeadm_worker_nodes)
* [Dependencies](#dependencies)
* [License](#license)
* [Author](#author)

---

## Default Variables

### kubeadm_apiserver_certsans

Extra sans for the apiserver

#### Default value

```YAML
kubeadm_apiserver_certsans: []
```

### kubeadm_apiserver_endpoint

Fixed endpoint for the controlplane

#### Default value

```YAML
kubeadm_apiserver_endpoint: 127.0.0.1:6443
```

#### Example usage

```YAML
kubeadm_apiserver_endpoint: kubernetes.example.com:6443
```

### kubeadm_bootstrap_expire

Expire duration for bootstrap token, never expires by default

#### Default value

```YAML
kubeadm_bootstrap_expire: 0
```

### kubeadm_bootstrap_token

Bootstrap tolken used for node init and join

#### Default value

```YAML
kubeadm_bootstrap_token:
```

#### Example usage

```YAML
kubeadm_bootstrap_token: abcdef.0123456789abcdef
```

### kubeadm_calico_manifests

List of manifests for calico networking

#### Default value

```YAML
kubeadm_calico_manifests:
  - https://docs.projectcalico.org/v{{ kubeadm_calico_version }}/manifests/calico.yaml
```

### kubeadm_calico_version

Version of calico manifest

#### Default value

```YAML
kubeadm_calico_version: 3.21
```

### kubeadm_canal_manifests

List of manifests for canal networking

#### Default value

```YAML
kubeadm_canal_manifests:
  - https://docs.projectcalico.org/v{{ kubeadm_canal_version }}/manifests/canal.yaml
```

### kubeadm_canal_version

Version of canal manifest

#### Default value

```YAML
kubeadm_canal_version: 3.21
```

### kubeadm_cloud_provider

#### Default value

```YAML
kubeadm_cloud_provider: external
```

### kubeadm_cluster_configuration

Kubeadm cluster configuration content

#### Default value

```YAML
kubeadm_cluster_configuration: "clusterName: {{ kubeadm_cluster_name }}\nkubernetesVersion:\
  \ stable-{{ kubeadm_kubernetes_version }}\ncontrolPlaneEndpoint: {{ kubeadm_apiserver_endpoint\
  \ }}\napiServer:\n  extraArgs: {{ kubeadm_default_apiserver_args | combine(kubeadm_extra_apiserver_args)\
  \ }}\n  certSANs: {{ kubeadm_apiserver_certsans | from_yaml }}\nscheduler:\n  extraArgs:\
  \ {{ kubeadm_default_scheduler_args | combine(kubeadm_extra_scheduler_args) }}\n\
  controllerManager:\n  extraArgs: {{ kubeadm_default_controller_args | combine(kubeadm_extra_controller_args)\
  \ }}\nnetworking:\n  serviceSubnet: 10.96.0.0/16\n  podSubnet: 10.244.0.0/16\n"
```

### kubeadm_cluster_name

Name of the Kubernetes cluster

#### Default value

```YAML
kubeadm_cluster_name: kubernetes
```

### kubeadm_cri_socket

Path to container runtime socket

#### Default value

```YAML
kubeadm_cri_socket: /run/containerd/containerd.sock
```

### kubeadm_default_apiserver_args

Default args for the apiserver

#### Default value

```YAML
kubeadm_default_apiserver_args:
  bind-address: 0.0.0.0
  cloud-provider: '{{ kubeadm_cloud_provider }}'
  authorization-mode: Node,RBAC
```

### kubeadm_default_controller_args

Default args for the controller

#### Default value

```YAML
kubeadm_default_controller_args:
  bind-address: 0.0.0.0
  cloud-provider: '{{ kubeadm_cloud_provider }}'
```

### kubeadm_default_kubelet_args

Default args for the kubelet

#### Default value

```YAML
kubeadm_default_kubelet_args:
  node-ip: '{{ kubeadm_local_address }}'
  cloud-provider: '{{ kubeadm_cloud_provider }}'
```

### kubeadm_default_scheduler_args

Default args for the scheduler

#### Default value

```YAML
kubeadm_default_scheduler_args:
  bind-address: 0.0.0.0
```

### kubeadm_extra_apiserver_args

Extra args for the apiserver

#### Default value

```YAML
kubeadm_extra_apiserver_args: {}
```

### kubeadm_extra_controller_args

Extra args for the controller

#### Default value

```YAML
kubeadm_extra_controller_args: {}
```

### kubeadm_extra_kubelet_args

Extra args for the kubelet

#### Default value

```YAML
kubeadm_extra_kubelet_args: {}
```

### kubeadm_extra_scheduler_args

Extra args for the scheduler

#### Default value

```YAML
kubeadm_extra_scheduler_args: {}
```

### kubeadm_flannel_manifests

List of manifests for flannel networking

#### Default value

```YAML
kubeadm_flannel_manifests:
  - https://raw.githubusercontent.com/flannel-io/flannel/v{{ kubeadm_flannel_version
    }}/Documentation/kube-flannel.yml
```

### kubeadm_flannel_version

Version of flannel manifest

#### Default value

```YAML
kubeadm_flannel_version: 0.16.1
```

### kubeadm_general_networking

List of manifests for general networking

#### Default value

```YAML
kubeadm_general_networking: []
```

### kubeadm_init_configuration

Kubeadm init configuration content

#### Default value

```YAML
kubeadm_init_configuration: "bootstrapTokens:\n  - token: {{ kubeadm_bootstrap_token\
  \ }}\n    ttl: \"{{ kubeadm_bootstrap_expire }}\"\n    usages:\n      - signing\n\
  \      - authentication\n    groups:\n      - system:bootstrappers:kubeadm:default-node-token\n\
  localAPIEndpoint:\n  advertiseAddress: {{ kubeadm_local_address }}\n  bindPort:\
  \ {{ kubeadm_local_port }}\nnodeRegistration:\n  criSocket: {{ kubeadm_cri_socket\
  \ }}\n  name: {{ inventory_hostname }}\n  kubeletExtraArgs: {{ kubeadm_default_kubelet_args\
  \ | combine(kubeadm_extra_kubelet_args) }}\n"
```

### kubeadm_join_configuration

Kubeadm join configuration content

#### Default value

```YAML
kubeadm_join_configuration: "discovery:\n  bootstrapToken:\n    apiServerEndpoint:\
  \ {{ kubeadm_apiserver_endpoint }}\n    token: {{ kubeadm_bootstrap_token }}\n \
  \   unsafeSkipCAVerification: True\n{% if inventory_hostname in kubeadm_master_nodes\
  \ %}\ncontrolPlane:\n  localAPIEndpoint:\n    advertiseAddress: {{ kubeadm_local_address\
  \ }}\n    bindPort: {{ kubeadm_local_port }}\n{% endif %}\nnodeRegistration:\n \
  \ criSocket: {{ kubeadm_cri_socket }}\n  name: {{ inventory_hostname }}\n  kubeletExtraArgs:\
  \ {{ kubeadm_default_kubelet_args | combine(kubeadm_extra_kubelet_args) }}\n"
```

### kubeadm_kubelet_configuration

Kubelet configuration content, optionally available

#### Default value

```YAML
kubeadm_kubelet_configuration:
```

### kubeadm_kubeproxy_configuration

Kubeproxy configuration content, optionally available

#### Default value

```YAML
kubeadm_kubeproxy_configuration:
```

### kubeadm_kubernetes_version

Vrsion of Kubernetes to install

#### Default value

```YAML
kubeadm_kubernetes_version: 1.23
```

### kubeadm_kuberouter_manifests

List of manifests for kube-router networking

#### Default value

```YAML
kubeadm_kuberouter_manifests:
  - https://raw.githubusercontent.com/cloudnativelabs/kube-router/v{{ kubeadm_kuberouter_version
    }}/daemonset/kubeadm-kuberouter-all-features.yaml
```

### kubeadm_kuberouter_version

Version of kube-router manifest

#### Default value

```YAML
kubeadm_kuberouter_version: 1.4.0
```

### kubeadm_local_address

Address to bind the controlplane to

#### Default value

```YAML
kubeadm_local_address: 127.0.0.1
```

### kubeadm_local_port

Port to bind the controlplane to

#### Default value

```YAML
kubeadm_local_port: 6443
```

### kubeadm_master_nodes

List of node names part of master role

#### Default value

```YAML
kubeadm_master_nodes: []
```

#### Example usage

```YAML
kubeadm_master_nodes:
  - master-01
  - master-02
  - master-03
```

### kubeadm_network_provider

Name of network provider to use, could be kuberouter, flannel, calico or canal

#### Default value

```YAML
kubeadm_network_provider: none
```

### kubeadm_worker_nodes

List of node names part of worker role

#### Default value

```YAML
kubeadm_worker_nodes: []
```

#### Example usage

```YAML
kubeadm_worker_nodes:
  - worker-01
  - worker-02
  - worker-03
```

## Dependencies

* None

## License

Apache-2.0

## Author

[Thomas Boerger](https://github.com/tboerger)
