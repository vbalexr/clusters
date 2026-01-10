# Setup of Magi

- Make a bond of the 2 2.5Gbps NICs(bond0)
    set ips 
        - 10.1.0.6/22
        - fd7a:6e5b:cafe:cafe:10::6/64
- Make a bridge of teh 2 10Gbps NICs(br0)
    set ips
        - 10.255.100.6/24
        - fd7a:6e5b:beff::6/64

# Update system
apt update
apt upgrade

# install some tools
apt install curl iputils-ping

# Gen token for k3s
openssl rand -base64 64
    save it somewhere safe

# on first node(balthasar) run

curl -sfL https://get.k3s.io | sh -s - server   \
    --cluster-init \
    --advertise-address=10.255.100.6 \
    --tls-san=10.1.0.5 \
    --tls-san=fd7a:6e5b:cafe:cafe:10::5 \
    --tls-san=magi.vbalex.com \
    --bind-address=0.0.0.0 \
    --flannel-iface=br0 \
    --flannel-ipv6-masq \
    --cluster-cidr=10.42.0.0/16,fd7a:6e5b:42::/56 \
    --service-cidr=10.43.0.0/16,fd7a:6e5b:43::/112 \
    --disable=servicelb \
    --disable=traefik \
    --disable=local-storage \
    --kubelet-arg containerd=/run/k3s/containerd/containerd.sock \
    --kube-controller-manager-arg=bind-address=0.0.0.0 \
    --kube-scheduler-arg=bind-address=0.0.0.0 \
    --kube-proxy-arg=metrics-bind-address=0.0.0.0 \
    --etcd-expose-metrics=true

Token for other nodes cat /var/lib/rancher/k3s/server/node-token
# on other nodes
export K3S_TOKEN="{NODE TOKEN}"

curl -sfL https://get.k3s.io | sh -s - server \
    --server https://magi.vbalex.com:6443 \
    --advertise-address=10.255.100.7 \
    --tls-san=10.1.0.5 \
    --tls-san=fd7a:6e5b:cafe:cafe:10::5 \
    --tls-san=magi.vbalex.com \
    --bind-address=0.0.0.0 \
    --flannel-iface=br0 \
    --flannel-ipv6-masq \
    --cluster-cidr=10.42.0.0/16,fd7a:6e5b:42::/56 \
    --service-cidr=10.43.0.0/16,fd7a:6e5b:43::/112 \
    --disable=servicelb \
    --disable=traefik \
    --disable=local-storage \
    --kubelet-arg containerd=/run/k3s/containerd/containerd.sock \
    --kube-controller-manager-arg=bind-address=0.0.0.0 \
    --kube-scheduler-arg=bind-address=0.0.0.0 \
    --kube-proxy-arg=metrics-bind-address=0.0.0.0 \
    --etcd-expose-metrics=true

curl -sfL https://get.k3s.io | sh -s - server \
    --server https://magi.vbalex.com:6443 \
    --advertise-address=10.255.100.8 \
    --tls-san=10.1.0.5 \
    --tls-san=fd7a:6e5b:cafe:cafe:10::5 \
    --tls-san=magi.vbalex.com \
    --bind-address=0.0.0.0 \
    --flannel-iface=br0 \
    --flannel-ipv6-masq \
    --cluster-cidr=10.42.0.0/16,fd7a:6e5b:42::/56 \
    --service-cidr=10.43.0.0/16,fd7a:6e5b:43::/112 \
    --disable=servicelb \
    --disable=traefik \
    --disable=local-storage \
    --kubelet-arg containerd=/run/k3s/containerd/containerd.sock \
    --kube-controller-manager-arg=bind-address=0.0.0.0 \
    --kube-scheduler-arg=bind-address=0.0.0.0 \
    --kube-proxy-arg=metrics-bind-address=0.0.0.0 \
    --etcd-expose-metrics=true


## Quick start
1) Create an age key locally
   age-keygen -o .age-key.txt

2) Bootstrap Flux (example: magi)
   flux bootstrap github \
     --components-extra=image-reflector-controller,image-automation-controller \
     --path clusters/magi \
     --branch main \
     --owner=vbalexr \
     --repository=clusters \
     --personal

3) Add SOPS key to the cluster (namespace: flux-system)
   kubectl -n flux-system create secret generic sops-age \
     --from-file=identity.agekey=.age-key.txt