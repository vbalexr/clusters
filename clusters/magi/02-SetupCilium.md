cilium install \
    --version 1.19.1 \
    --set kubeProxyReplacement=true \
    --set k8sServiceHost=magi.vbalex.com \
    --set k8sServicePort=6443 \
    --set ipv4.enabled=true \
    --set ipv6.enabled=true \
    --set ipam.mode=kubernetes \
    --set cni.exclusive=false \
    --set bgpControlPlane.enabled=true \
    --set cni.binPath=/var/lib/rancher/k3s/data/cni/ \
    --set cni.confPath=/var/lib/rancher/k3s/agent/etc/cni/net.d

cilium status --wait