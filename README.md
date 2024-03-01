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
helm repo update
helm install --namespace argo-cd --create-namespace argo-cd applications/argo-cd/
kubectl get pods -n argo-cd --watch
```
- Wait for all pods to be in Running state


Get initial secret:
```bash
kubectl get secret -n argo-cd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Forward Port to 8080 (we'll add ingress-controller later):
```bash
kubectl port-forward -n argo-cd svc/argo-cd-argocd-server 8080:443
```

## Apply Management App
```bash
helm template app-of-apps/ | kubectl apply -f -
```

## Install Charts for applications
```bash
helm dependency build
```

## Debugging

**Apps Hanging On Delete**
https://argo-cd.readthedocs.io/en/stable/user-guide/app_deletion/