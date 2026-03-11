´´´bash
export NODE_IP4="10.255.0.6"
export NODE_IP6="fd7a:cafe::6"
export NODE_EXTERNAL_IP4="10.1.0.6"
export NODE_EXTERNAL_IP6="fd7a:6e5b:cafe:10::6"

export API_VIP="10.1.0.5"
export API_DNS="magi.vbalex.com"
export INTERNAL_API_VIP="10.255.0.5"

curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="\
server \
--cluster-init \
--node-ip=${NODE_IP4},${NODE_IP6} \
--node-external-ip=${NODE_EXTERNAL_IP4},${NODE_EXTERNAL_IP6} \
--advertise-address=${NODE_IP4} \
--tls-san=${API_VIP} \
--tls-san=${API_DNS} \
--tls-san=${INTERNAL_API_VIP} \
--flannel-backend=none \
--disable-network-policy \
--disable=traefik \
--disable=servicelb \
--disable=local-storage \
--cluster-cidr=10.42.0.0/16,fd7a:42::/56 \
--service-cidr=10.43.0.0/16,fd7a:43::/112 \
--write-kubeconfig-mode=644 \
" sh -


sudo cat /var/lib/rancher/k3s/server/node-token
´´´

´´´bash
export NODE_IP4="10.255.0.7"
export NODE_IP6="fd7a:cafe::7"
export NODE_EXTERNAL_IP4="10.1.0.7"
export NODE_EXTERNAL_IP6="fd7a:6e5b:cafe:10::7"

export API_VIP="10.1.0.5"
export API_DNS="magi.vbalex.com"
export INTERNAL_API_VIP="10.255.0.5"

export TOKEN="K107bca61a92b83a10e0ae194710b425798f559a62bef8aeca5c900225170327ee1::server:0982140d19dce4b9b2f6ac40bb2db1b2"

curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="\
server \
--server https://${INTERNAL_API_VIP}:6443 \
--token=${TOKEN} \
--node-ip=${NODE_IP4},${NODE_IP6} \
--node-external-ip=${NODE_EXTERNAL_IP4},${NODE_EXTERNAL_IP6} \
--advertise-address=${NODE_IP4} \
--tls-san=${API_VIP} \
--tls-san=${API_DNS} \
--tls-san=${INTERNAL_API_VIP} \
--flannel-backend=none \
--disable-network-policy \
--disable=traefik \
--disable=servicelb \
--disable=local-storage \
--cluster-cidr=10.42.0.0/16,fd7a:42::/56 \
--service-cidr=10.43.0.0/16,fd7a:43::/112 \
--write-kubeconfig-mode=644 \
" sh -
´´´


´´´bash
export NODE_IP4="10.255.0.8"
export NODE_IP6="fd7a:cafe::8"
export NODE_EXTERNAL_IP="10.1.0.8"
export NODE_EXTERNAL_IP6="fd7a:6e5b:cafe:10::8"

export API_VIP="10.1.0.5"
export API_DNS="magi.vbalex.com"
export INTERNAL_API_VIP="10.255.0.5"

export TOKEN="K107bca61a92b83a10e0ae194710b425798f559a62bef8aeca5c900225170327ee1::server:0982140d19dce4b9b2f6ac40bb2db1b2"

curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="\
server \
--server https://${INTERNAL_API_VIP}:6443 \
--token=${TOKEN} \
--node-ip=${NODE_IP4},${NODE_IP6} \
--node-external-ip=${NODE_EXTERNAL_IP},${NODE_EXTERNAL_IP6} \
--advertise-address=${NODE_IP4} \
--tls-san=${API_VIP} \
--tls-san=${API_DNS} \
--tls-san=${INTERNAL_API_VIP} \
--flannel-backend=none \
--disable-network-policy \
--disable=traefik \
--disable=servicelb \
--disable=local-storage \
--cluster-cidr=10.42.0.0/16,fd7a:42::/56 \
--service-cidr=10.43.0.0/16,fd7a:43::/112 \
--write-kubeconfig-mode=644 \
" sh -
´´´