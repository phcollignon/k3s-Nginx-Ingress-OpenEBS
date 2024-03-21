
# k3s-Nginx-Ingress-OpenEBS


K3s is a lightweight implementation of Kubernetes, particularly in terms of I/O disk writes, making it quite convenient for a homelab setup. However, I found it lacking in two main areas:
- Nginx ingress (since it uses Traefik by default)
- A customizable hostpath-based storage provider to store data on multiple NAS disks.
  
Here is how to install K3s with Nginx ingress and [OpenEBS](https://openebs.io) storage:

Install k3s and disable Traefik :
```
curl -sfL https://get.k3s.io | sh -s -
sudo rm -rf /var/lib/rancher/k3s/server/manifests/traefik.yaml
helm uninstall traefik traefik-crd -n kube-system
sudo systemctl restart k3s
```

Install Nginx ingress :
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install my-nginx-ingress ingress-nginx/ingress-nginx --namespace ingress-nginx --create-namespace
```

Install OpenEBS : 
```
helm repo add openebs https://openebs.github.io/charts
helm repo update
helm install openebs --namespace openebs openebs/openebs --create-namespace
```

and finally add `ingressClassName: nginx` to your ingress spec 
