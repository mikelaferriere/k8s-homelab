# k8s-homelab
k8s helm deployments

*References*
- https://www.arthurkoziel.com/setting-up-argocd-with-helm/
- https://medium.com/@wadexu007/unlocking-the-power-of-argocd-mastering-app-of-apps-for-seamless-application-management-9c8bc78242fd

## Initialization
Inside SIDA, run

SIDA/tf:
```bash
terraform apply
```
  - To create the VMs

SIDA/code_deploy:
```bash
ansible-playbook -i inv/homelab k8s-control-plane-yml
```
  - This will create the control plane

```bash
ansible-playbook -i inv/homelab k8s-worker.yml
```
  - This will create worker nodes, and join them to the control plane

### Install ArgoCD on cluster
Inside this repo, run:
```bash
helm repo add argo-cd https://argoproj.github.io/argo-helm
helm install --namespace argocd --create-namespace argo-cd charts/argo-cd/
kubectl get pods --watch
```
- Wait for all pods to be in Running state


Get initial secret:
```bash
kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Forward Port to 8080 (we'll add ingress-controller later):
```bash
kubectl port-forward svc/argo-cd-argocd-server 8080:443
```

## Apply Management App
```bash
helm template management/ | kubectl apply -f -
```