# Create configmaps
```
kubectl -n vault-unlock create configmap consul --from-file=config/consul.json

kubectl -n vault-unlock create configmap vault --from-file=config/vault.json
```

# Update configmaps
```
kubectl -n vault-unlock create configmap consul --from-file=config/consul.json -o yaml --dry-run | kubectl replace -f -

kubectl -n vault-unlock create configmap vault --from-file=config/vault.json -o yaml --dry-run | kubectl replace -f -
```

# replace configmap vault with vault_autounseal.json
```
kubectl -n vault-unlock create configmap vault --from-file=vault.json=config/vault_autounseal.json -o yaml --dry-run | kubectl replace -f -
```
