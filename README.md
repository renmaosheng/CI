# k8s-network-policy
This records the experiment by testing flannel with calico network policy using canal. 
Please always refer to https://github.com/tigera/canal as the original. 

If you are using kargo to deploy k8s with flannel, this readme can help you manually set
up the calico network policy. 

1. change kubelet.env to allow cni.
2. adding cni-v0.3.0.tgz into /opt/cni/bin, as some k8s version doesn't have it.
3. nsenter is not installed sometimes, we need to copy is in /bin of work node. 
4. kubectl create -f canal-no-flannel.yaml
5. remove the ""-A POSTROUTING -s 10.233.62.0/24 ! -o docker0 -j MASQUERADE" created by
   docker. As felix read pod IP to filter, but that rule will make all the src POD IPs 
   to snat to flannel.1' IP which breaks the felix functionality.

The test directory has some yaml to validate k8s network policy function. 

Todo:
1. contribute to kargo
