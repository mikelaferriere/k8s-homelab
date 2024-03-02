https://github.com/grafana/loki/tree/main/production/helm/loki
https://grafana.com/docs/loki/next/setup/install/helm/
https://grafana.com/docs/loki/latest/setup/install/helm/install-monolithic/
https://grafana.com/docs/loki/latest/operations/storage/filesystem/
https://github.com/grafana/loki/issues/7047

# Get admin password
```bash
kubectl get secret --namespace monitoring loki -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```