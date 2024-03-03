https://docs.goauthentik.io/docs/installation/kubernetes
https://github.com/goauthentik/helm/tree/main/charts/authentik

# Setup
Create secret key and postgres password in hashicorp vault
- secret-key: kv/authentik
  key: secret-key
  value: <50charsecret>

- secret-key: kv/authentik/postgresql
  key: password
  value: <40charsecret>
