# K3S Single Node Cluster (MACOS)

## Install multipass and k3s inside that VM
```bash
$ brew cask install multipass
$ multipass launch --name k3s --cpus 4 --mem 4g --disk 20g
$ multipass exec k3s -- bash -c "curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -"
```

## Access K8s cluster from local

```bash
$ multipass info k3s
$ K3S_IP=$(multipass info k3s | grep IPv4 | awk '{print $2}')
$ multipass exec k3s sudo cat /etc/rancher/k3s/k3s.yaml > k3s.yaml
$ sed -i '' "s/127.0.0.1/${K3S_IP}/" k3s.yaml
$ export KUBECONFIG=${PWD}/k3s.yaml
```

## Verify Cluster Installation

```bash
$ kubectl get nodes
```

## Directory Share between host and VM

```bash
$ multipass exec k3s -- mkdir /home/ubuntu/code
$ multipass mount ~/code k3s:/home/ubuntu/code # multipass mount [local_src_dir] [vm_name:/target/path/inside_vm]
```

## Verify if the mount works

```bash
$ multipass info k3s
```

## Directory Umount

```bash
$ multipass umount k3s:/home/ubuntu/code # multipass mount [vm_name:/target/path/inside_vm]
```

## Verify if the umount works

```bash
$ multipass info k3s
```

## Pass  a cloud-init metadata file to an instance on launch

```bash
$ multipass launch --name k3s --cpus 4 --mem 4g --disk 20g --cloud-init cloud-config.yaml
```

## Run a command inside a VM

```bash
$ multipass exec k3s -- lsb_release -a
```

## List if useful command

```bash
$ multipass list
$ multipass stop k3s
$ multipass start k3s
$ multipass delete k3s
$ multipass purge
```



