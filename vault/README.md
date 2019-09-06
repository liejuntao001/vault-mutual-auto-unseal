# Create configmaps
```
kubectl -n vault create configmap consul --from-file=config/consul.json

kubectl -n vault create configmap vault --from-file=config/vault.json
```

# Update configmaps
```
kubectl -n vault create configmap consul --from-file=config/consul.json -o yaml --dry-run | kubectl replace -f -

kubectl -n vault create configmap vault --from-file=config/vault.json -o yaml --dry-run | kubectl replace -f -
```

# replace configmap vault with vault_autounseal.json
```
kubectl -n vault create configmap vault --from-file=vault.json=config/vault_autounseal.json -o yaml --dry-run | kubectl replace -f -
```

# Add ingress
```
DOMAIN=k8s.example.com envsubst < vault_ingress.yml | kubectl apply -f -
```

# Switch to emergency unseal
```
kubectl -n vault create configmap vault --from-file=vault.json=config/vault_emergency.json -o yaml --dry-run | kubectl replace -f -
kubectl apply -f vault_deployment_emergency.yml
```

# Switch to auto unseal
```
kubectl -n vault create configmap vault --from-file=vault.json=config/vault_autounseal.json -o yaml --dry-run | kubectl replace -f -
kubectl apply -f vault_deployment.yml
```

