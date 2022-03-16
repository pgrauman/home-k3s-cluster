<!-- taken from https://github.com/toboshii/home-cluster/ -->
<img src="https://camo.githubusercontent.com/5b298bf6b0596795602bd771c5bddbb963e83e0f/68747470733a2f2f692e696d6775722e636f6d2f7031527a586a512e706e67" align="left" width="144px" height="144px"/>

# :sailboat: Home Kubernetes Cluster
_ k3s managed by Flux

# :notebook_with_decorative_cover: Description
Kubernetes cluster running at home used as a k8s learning lab and home content server. The nodes are mixed architecture and running k3s.

Based on the [k8s-at-home k3 template](https://github.com/k8s-at-home/template-cluster-k3s)

---

# :hammer_and_wrench: Hardware

| Type      | Quantity | Arch  | RAM | CPU |
|-----------|----------|-------|-----|-----|
| Ubuntu VM | 2        | amd64 | 14  | 4   |
| RPi 3b    | 3        | arm64 | 12  | 6   |

---

## :sparkles:&nbsp; Cluster setup

This cluster consists of VMs provisioned on [PVE](https://www.proxmox.com/en/proxmox-ve) via the [Terraform Proxmox provider](https://github.com/Telmate/terraform-provider-proxmox). These run [k3s](https://k3s.io/) provisioned overtop Ubuntu 20.10 using the [Ansible](https://www.ansible.com/) galaxy role [ansible-role-k3s](https://github.com/PyratLabs/ansible-role-k3s). This cluster is not hyper-converged as block storage is provided by the underlying PVE Ceph cluster using rook-ceph-external.

See my [server/ansible](./server/ansible/) directory for my playbooks and roles, and [server/terraform](./server/terraform) for infrastructure provisioning.

## :art:&nbsp; Cluster components

- [kube-vip](https://kube-vip.io/): Uses BGP to load balance the control-plane API, making it highly availible without requiring external HA proxy solutions.
- [calico](https://docs.projectcalico.org/about/about-calico): For internal cluster networking using BGP.
- [traefik](https://traefik.io/): Provides ingress cluster services.
- [SOPS](https://toolkit.fluxcd.io/guides/mozilla-sops/): Encrypts secrets which is safe to store - even to a public repository.
- [external-dns](https://github.com/kubernetes-sigs/external-dns): Creates DNS entries in a separate [coredns](https://github.com/coredns/coredns) deployment which is backed by my clusters [etcd](https://github.com/etcd-io/etcd) deployment.
- [cert-manager](https://cert-manager.io/docs/): Configured to create TLS certs for all ingress services automatically using LetsEncrypt.

(section taken from [toshobi's cluster repo](https://github.com/toboshii/home-cluster/), which has great descriptions)

---

# :handshake: Thanks

Thanks to the [k8s-at-home community](https://k8s-at-home.com/) for putting out great resources, examples, and for being such a friendly, helpful community.
