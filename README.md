# kubeadm

[![Source Code](https://img.shields.io/badge/github-source%20code-blue?logo=github&logoColor=white)](https://github.com/rolehippie/kubeadm)
[![General Workflow](https://github.com/rolehippie/kubeadm/actions/workflows/general.yml/badge.svg)](https://github.com/rolehippie/kubeadm/actions/workflows/general.yml)
[![Readme Workflow](https://github.com/rolehippie/kubeadm/actions/workflows/docs.yml/badge.svg)](https://github.com/rolehippie/kubeadm/actions/workflows/docs.yml)
[![Galaxy Workflow](https://github.com/rolehippie/kubeadm/actions/workflows/galaxy.yml/badge.svg)](https://github.com/rolehippie/kubeadm/actions/workflows/galaxy.yml)
[![License: Apache-2.0](https://img.shields.io/github/license/rolehippie/kubeadm)](https://github.com/rolehippie/kubeadm/blob/master/LICENSE)
[![Ansible Role](https://img.shields.io/badge/role-rolehippie.kubeadm-blue)](https://galaxy.ansible.com/rolehippie/kubeadm)

Ansible role to install kubeadm and bootstrap single node Kubernetes.

## Sponsor

Building and improving this Ansible role have been sponsored by my current and previous employers like **[Cloudpunks GmbH](https://cloudpunks.de)** and **[Proact Deutschland GmbH](https://www.proact.eu)**.

## Table of content

- [Requirements](#requirements)
- [Default Variables](#default-variables)
  - [kubeadm_apiserver_certsans](#kubeadm_apiserver_certsans)
  - [kubeadm_apiserver_endpoint](#kubeadm_apiserver_endpoint)
  - [kubeadm_calico_manifests](#kubeadm_calico_manifests)
  - [kubeadm_calico_version](#kubeadm_calico_version)
  - [kubeadm_canal_manifests](#kubeadm_canal_manifests)
  - [kubeadm_canal_version](#kubeadm_canal_version)
  - [kubeadm_cluster_config](#kubeadm_cluster_config)
  - [kubeadm_cluster_name](#kubeadm_cluster_name)
  - [kubeadm_cri_socket](#kubeadm_cri_socket)
  - [kubeadm_default_apiserver_args](#kubeadm_default_apiserver_args)
  - [kubeadm_default_controller_args](#kubeadm_default_controller_args)
  - [kubeadm_default_scheduler_args](#kubeadm_default_scheduler_args)
  - [kubeadm_extra_apiserver_args](#kubeadm_extra_apiserver_args)
  - [kubeadm_extra_controller_args](#kubeadm_extra_controller_args)
  - [kubeadm_extra_scheduler_args](#kubeadm_extra_scheduler_args)
  - [kubeadm_flannel_manifests](#kubeadm_flannel_manifests)
  - [kubeadm_flannel_version](#kubeadm_flannel_version)
  - [kubeadm_general_networking](#kubeadm_general_networking)
  - [kubeadm_init_config](#kubeadm_init_config)
  - [kubeadm_keyring](#kubeadm_keyring)
  - [kubeadm_kubelet_config](#kubeadm_kubelet_config)
  - [kubeadm_kubelet_config_enabled](#kubeadm_kubelet_config_enabled)
  - [kubeadm_kubeproxy_config](#kubeadm_kubeproxy_config)
  - [kubeadm_kubeproxy_config_enabled](#kubeadm_kubeproxy_config_enabled)
  - [kubeadm_kubernetes_version](#kubeadm_kubernetes_version)
  - [kubeadm_local_address](#kubeadm_local_address)
  - [kubeadm_local_port](#kubeadm_local_port)
  - [kubeadm_network_provider](#kubeadm_network_provider)
  - [kubeadm_pod_subnet](#kubeadm_pod_subnet)
  - [kubeadm_service_subnet](#kubeadm_service_subnet)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Requirements

- Minimum Ansible version: `2.10`

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

### kubeadm_cluster_config

Kubeadm cluster configuration content

#### Default value

```YAML
kubeadm_cluster_config: |
  apiVersion: kubeadm.k8s.io/v1beta3
  kind: ClusterConfiguration
  clusterName: {{ kubeadm_cluster_name }}
  kubernetesVersion: stable-{{ kubeadm_kubernetes_version }}
  controlPlaneEndpoint: {{ kubeadm_apiserver_endpoint }}
  apiServer:
    extraArgs: {{ kubeadm_default_apiserver_args | combine(kubeadm_extra_apiserver_args) }}
    certSANs: {{ kubeadm_apiserver_certsans | from_yaml }}
  scheduler:
    extraArgs: {{ kubeadm_default_scheduler_args | combine(kubeadm_extra_scheduler_args) }}
  controllerManager:
    extraArgs: {{ kubeadm_default_controller_args | combine(kubeadm_extra_controller_args) }}
  networking:
    serviceSubnet: {{ kubeadm_service_subnet }}
    podSubnet: {{ kubeadm_pod_subnet }}
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
  cloud-provider: external
  authorization-mode: Node,RBAC
```

### kubeadm_default_controller_args

Default args for the controller

#### Default value

```YAML
kubeadm_default_controller_args:
  bind-address: 0.0.0.0
  cloud-provider: external
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

### kubeadm_init_config

Kubeadm init configuration content

#### Default value

```YAML
kubeadm_init_config: |
  apiVersion: kubeadm.k8s.io/v1beta3
  kind: InitConfiguration
  localAPIEndpoint:
    advertiseAddress: {{ kubeadm_local_address }}
    bindPort: {{ kubeadm_local_port }}
  nodeRegistration:
    criSocket: {{ kubeadm_cri_socket }}
    name: {{ inventory_hostname }}
```

### kubeadm_keyring

Path for the repository keyring

#### Default value

```YAML
kubeadm_keyring: /usr/share/keyrings/kubernetes-archive-keyring.gpg
```

### kubeadm_kubelet_config

Kubelet configuration content, optionally available

#### Default value

```YAML
kubeadm_kubelet_config: |
  apiVersion: kubelet.config.k8s.io/v1beta1
  kind: KubeletConfiguration
```

### kubeadm_kubelet_config_enabled

Enable the kublet configuration

#### Default value

```YAML
kubeadm_kubelet_config_enabled: false
```

### kubeadm_kubeproxy_config

Kubeproxy configuration content, optionally available

#### Default value

```YAML
kubeadm_kubeproxy_config: |
  apiVersion: kubeproxy.config.k8s.io/v1alpha1
  kind: KubeProxyConfiguration
```

### kubeadm_kubeproxy_config_enabled

Enable the kubeproxy configuration

#### Default value

```YAML
kubeadm_kubeproxy_config_enabled: false
```

### kubeadm_kubernetes_version

Vrsion of Kubernetes to install

#### Default value

```YAML
kubeadm_kubernetes_version: 1.23
```

### kubeadm_local_address

Address to bind the controlplane to

#### Default value

```YAML
kubeadm_local_address: 0.0.0.0
```

### kubeadm_local_port

Port to bind the controlplane to

#### Default value

```YAML
kubeadm_local_port: 6443
```

### kubeadm_network_provider

Name of network provider to use, could be kuberouter, flannel, calico or canal

#### Default value

```YAML
kubeadm_network_provider: none
```

### kubeadm_pod_subnet

Used subnet for pods

#### Default value

```YAML
kubeadm_pod_subnet: 10.244.0.0/16
```

### kubeadm_service_subnet

Used subnet for services

#### Default value

```YAML
kubeadm_service_subnet: 10.96.0.0/16
```

## Discovered Tags

**_kubeadm_**


## Dependencies

- None

## License

Apache-2.0

## Author

[Thomas Boerger](https://github.com/tboerger)
