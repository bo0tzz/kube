# Node installation
First node:
```
ssh ubuntu@192.168.4.2 'echo "PubkeyAcceptedKeyTypes=+ssh-rsa" | sudo tee -a /etc/ssh/sshd_config && sudo systemctl restart sshd'
k3sup install --ip 192.168.4.2 --tls-san 192.168.4.1 --user ubuntu --cluster --local-path ~/.kube/config --merge --context new-cluster --k3s-extra-args "--no-deploy servicelb --no-deploy traefik"
kubectl apply -f infrastructure/kube-vip/
```
Edit kubeconfig to use loadbalanced IP
Add other nodes
```
for ip in 192.168.4.{3..4}; do
  ssh ubuntu@$ip 'echo "PubkeyAcceptedKeyTypes=+ssh-rsa" | sudo tee -a /etc/ssh/sshd_config && sudo systemctl restart sshd'
  k3sup join --ip $ip --user ubuntu --server-user ubuntu --server-host 192.168.4.1 --server --k3s-extra-args "--no-deploy servicelb --no-deploy traefik"
done
```
