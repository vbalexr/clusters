
# Gen token for k3s
openssl rand -base64 64
    save it somewhere safe

# on first node(balthasar) run

curl -sfL https://get.k3s.io | sh -s - server \
    --cluster-init \
    --node-ip=10.255.0.6,fd7a:cafe::6 \
    --node-external-ip=10.1.0.6,fd7a:6e5b:cafe:10::6 \
    --advertise-address=10.255.0.6 \
    --flannel-backend=none \
    --disable-network-policy \
    --tls-san=10.1.0.5 \
    --tls-san=fd7a:6e5b:cafe:10::5 \
    --tls-san=magi.vbalex.com \
    --cluster-cidr=10.42.0.0/16,fd7a:6e5b:42::/56 \
    --service-cidr=10.43.0.0/16,fd7a:6e5b:43::/112 \
    --disable=servicelb \
    --disable=traefik \
    --disable=local-storage \
    --kube-controller-manager-arg=bind-address=0.0.0.0 \
    --kube-scheduler-arg=bind-address=0.0.0.0 \
    --etcd-expose-metrics=true

Token for other nodes cat /var/lib/rancher/k3s/server/node-token
# on other nodes
export K3S_TOKEN="{NODE TOKEN}"

curl -sfL https://get.k3s.io | sh -s - server \
    --server https://magi.vbalex.com:6443 \
    --node-ip=10.255.0.7,fd7a:cafe::7 \
    --node-external-ip=10.1.0.7,fd7a:6e5b:cafe:10::7 \
    --advertise-address=10.255.0.7 \
    --flannel-backend=none \
    --disable-network-policy \
    --tls-san=10.1.0.5 \
    --tls-san=fd7a:6e5b:cafe:10::5 \
    --tls-san=magi.vbalex.com \
    --cluster-cidr=10.42.0.0/16,fd7a:6e5b:42::/56 \
    --service-cidr=10.43.0.0/16,fd7a:6e5b:43::/112 \
    --disable=servicelb \
    --disable=traefik \
    --disable=local-storage \
    --kube-controller-manager-arg=bind-address=0.0.0.0 \
    --kube-scheduler-arg=bind-address=0.0.0.0 \
    --etcd-expose-metrics=true

curl -sfL https://get.k3s.io | sh -s - server \
    --server https://magi.vbalex.com:6443 \
    --node-ip=10.255.0.8,fd7a:cafe::8 \
    --node-external-ip=10.1.0.8,fd7a:6e5b:cafe:10::8 \
    --advertise-address=10.255.0.8 \
    --flannel-backend=none \
    --disable-network-policy \
    --tls-san=10.1.0.5 \
    --tls-san=fd7a:6e5b:cafe:10::5 \
    --tls-san=magi.vbalex.com \
    --cluster-cidr=10.42.0.0/16,fd7a:6e5b:42::/56 \
    --service-cidr=10.43.0.0/16,fd7a:6e5b:43::/112 \
    --disable=servicelb \
    --disable=traefik \
    --disable=local-storage \
    --kube-controller-manager-arg=bind-address=0.0.0.0 \
    --kube-scheduler-arg=bind-address=0.0.0.0 \
    --etcd-expose-metrics=true