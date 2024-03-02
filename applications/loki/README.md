https://grafana.com/docs/loki/next/setup/install/helm/

# Get admin password
```bash
kubectl get secret --namespace monitoring loki -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```