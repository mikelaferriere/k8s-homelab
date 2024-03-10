https://developer.hashicorp.com/vault/docs/platform/k8s/helm
https://github.com/hashicorp/vault-helm
https://github.com/hashicorp/vault-helm/blob/main/values.yaml


# Bugs
Secrets with `-` in the name:
https://support.hashicorp.com/hc/en-us/articles/15667022913299-Vault-Agent-throws-bad-character-U-002D


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

# Create key/value secrets
https://developer.hashicorp.com/vault/docs/secrets/kv/kv-v2

```bash
kubectl -n security exec -it hashicorp-vault-0 -- /bin/sh
/ $

export VAULT_TOKEN=<root-token>

vault secrets enable -version=2 kv
vault kv put kv/database/config username="db-readonly-username" password="db-secret-password"
vault kv get kv/database/config
```

# Enable AppRole auth so other apps in the cluster can access secrets
https://developer.hashicorp.com/vault/docs/auth/approle

```bash
vault auth enable approle

vault write auth/approle/role/app \
    secret_id_ttl=10m \
    token_num_uses=10 \
    token_ttl=20m \
    token_max_ttl=30m \
    secret_id_num_uses=40

```

# Configure Kubernetes authentication
https://developer.hashicorp.com/vault/tutorials/kubernetes/kubernetes-sidecar
```bash
vault auth enable kubernetes
vault write auth/kubernetes/config \
      kubernetes_host="https://$KUBERNETES_PORT_443_TCP_ADDR:443"
```

# Create and attach policy to kubernetes
https://www.hashicorp.com/resources/policies-vault

```bash
vault policy write internal-app - <<EOF
path "*" {
   capabilities = ["read"]
}
EOF

vault write auth/kubernetes/role/internal-app \
      bound_service_account_names=* \
      bound_service_account_namespaces=* \
      policies=internal-app \
      ttl=1h
```

# Configure OIDC
```bash
export VAULT_TOKEN=<root-token>
export VAULT_ADDR='http://127.0.0.1:8200'

vault auth enable oidc

vault write auth/oidc/config \
    oidc_discovery_url="https://<EXTERNAL_OATUH_DOMAIN>/application/o/hashicorp-vault/" \
    oidc_client_id="<HASHICORP_CLIENT_ID>" \
    oidc_client_secret="<HASHICORP_CLIENT_SECRET>" \
    default_role="reader"

vault write auth/oidc/role/reader \
    bound_audiences="<HASHICORP_CLIENT_ID>" \
    allowed_redirect_uris="https://<EXTERNAL_DOMAIN>/ui/vault/auth/oidc/oidc/callback" \
    allowed_redirect_uris="https://<EXTERNAL_DOMAIN>/oidc/callback" \
    allowed_redirect_uris="http://localhost:8200/oidc/callback" \
    user_claim="sub" \
    policies="reader"

vault login -method=oidc role="reader"
```

# Secret Store CSI
For when using attributes won't work:
https://developer.hashicorp.com/vault/tutorials/kubernetes/kubernetes-secret-store-driver