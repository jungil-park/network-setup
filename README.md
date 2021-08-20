## 1. Provision :
 

 😺

My favorite search engine is [Duck Duck Go](https://duckduckgo.com "The best search engine for privacy").

 
* aws :
        ansible-playbook -e "mode=apply cloud_type=aws env=aws" provcluster.yml
 
        mode=apply      :  apply or destroy
        cloud_type=aws  : Ansible script location (ROLE)
        env=aws         : configuration (VAR)
 
        validate : /opt/cello/src/agent/ansible/run/runhosts
                                   /etc/hosts
 
2. Init N/W :  --tags "copy flannel package"
 
aws :
        ansible-playbook -i run/runhosts -e "mode=apply env_type=flanneld env=aws" initcluster.yml
        ansible-playbook -i run/runhosts -e "mode=apply env_type=k8s env=aws" initcluster.yml
 
 
                ansible-playbook -i run/runhosts -e "mode=apply env_type=flanneld env=vb" initcluster.yml
 
 
 
 
 
kubectl uses the default kubeconfig file, $HOME/.kube/config
 
Dashboard :   public IP + proxy port  (apiserver) ????
http://52.28.246.4:8080/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/overview?namespace=default
 
 
 
3. Deploy fabric network
 
aws :
           ansible-playbook -i run/runhosts -e "mode=apply deploy_type=compose env=bc1st" setupfabric.yml
 
 
           ansible-playbook -i run/runhosts -e "mode=apply deploy_type=k8s env=bc1st" setupfabric.yml
           ansible-playbook -i run/runhosts -e "mode=apply deploy_type=compose env=vb1st" setupfabric.yml
 
 
         generate crypto and genesis :   /opt/gopath/bc1st/fabric
 
 
 
ansible-playbook -i run/runhosts -e "mode=apply env=aws" setupclient.yml
 
 
 
 
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 
 
172.31.13.54
 
export PATH=$PATH:/opt/fabric/bin
etcdctl  ls --recursive
 
etcdctl --endpoint http://172.31.13.54:4002  ls --recursive
etcdctl --endpoint http://172.31.13.54:4002  get /skydns/config
 
docker inspect -f '{{ .NetworkSettings.IPAddress }}' peer1st-orgb
etcdctl get /skydnscnet/orderer2nd-orgc
 
 
docker run -d --name test-etcd -p 4002:4002 quay.io/coreos/etcd:v3.0.2 --listen-client-urls=http://0.0.0.0:4002 --advertise-client-urls=http://172.31.13.54:4002
 
  docker exec test-etcd etcdctl --endpoint http://172.31.13.54:4002 cluster-health
docker exec test-etcd etcdctl --endpoint http://172.31.13.54:4002 set /skydnsg '{"dns_addr":"0.0.0.0:53", "ttl":60, "domain": "cluster.local.", "nameservers": ["8.8.8.8:53","8.8.4.4:53"]}'
 
docker run -d --net host --name test-skydns -e ETCD_MACHINES=http://172.31.13.54:4002 skynetservices/skydns:2.5.3a