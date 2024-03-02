https://developer.hashicorp.com/vault/docs/platform/k8s/helm
https://github.com/hashicorp/vault-helm
https://github.com/hashicorp/vault-helm/blob/main/values.yaml

# Unseal vault

## Get vault pods
```bash
kubectl get pods -l app.kubernetes.io/name=vault -n security
```

## Get unseal keys
```bash
kubectl -n security exec -ti hashicorp-vault-0 -- vault operator init
```

## Unseal vault
```bash
kubectl -n security exec -ti hashicorp-vault-0 -- vault operator unseal <unseal-key>
kubectl -n securityexec -ti hashicorp-vault-0 -- vault operator unseal <unseal-key>
kubectl -n security exec -ti hashicorp-vault-0 -- vault operator unseal <unseal-key>
```