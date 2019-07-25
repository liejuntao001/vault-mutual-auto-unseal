# Generate CA
```
cfssl gencert -initca ca/ca-csr.json | cfssljson -bare ca
```
# Generate certs
```
cfssl gencert \
    -ca=ca.pem \
    -ca-key=ca-key.pem \
    -config=config/ca-config.json \
    -profile=default \
    config/consul-csr.json | cfssljson -bare consul
```

```
cfssl gencert \
    -ca=ca.pem \
    -ca-key=ca-key.pem \
    -config=config/ca-config.json \
    -profile=default \
    config/vault-csr.json | cfssljson -bare vault
```

```
cfssl gencert \
    -ca=ca.pem \
    -ca-key=ca-key.pem \
    -config=config/ca-config.json \
    -profile=default \
    config/vault-unlock-csr.json | cfssljson -bare vault-unlock
```


# Generate gossip key
```
export GOSSIP_ENCRYPTION_KEY=$(consul keygen)
```

# consul 
```
kubectl -n consul create secret generic consul \
    --from-literal="gossip-encryption-key=${GOSSIP_ENCRYPTION_KEY}" \
    --from-file=ca.pem \
    --from-file=consul.pem \
    --from-file=consul-key.pem
```

# vault
```
kubectl -n vault create secret generic vault \
    --from-literal="gossip-encryption-key=${GOSSIP_ENCRYPTION_KEY}" \
    --from-file=ca.pem \
    --from-file=vault.pem \
    --from-file=vault-key.pem
```

# vault-unlock
```
kubectl -n vault-unlock create secret generic vault \
    --from-literal="gossip-encryption-key=${GOSSIP_ENCRYPTION_KEY}" \
    --from-file=ca.pem \
    --from-file=vault-unlock.pem \
    --from-file=vault-unlock-key.pem \
    --from-file=vault-client.pem \
    --from-file=vault-client-key.pem
```


# Backup Cluster

```
cfssl gencert \
    -ca=ca.pem \
    -ca-key=ca-key.pem \
    -config=config/ca-config.json \
    -profile=default \
    config/consul-back-csr.json | cfssljson -bare consul-back
```

## consul-back
```
kubectl -n consul-back create secret generic consul \
    --from-literal="gossip-encryption-key=${GOSSIP_ENCRYPTION_KEY}" \
    --from-file=ca.pem \
    --from-file=consul-back.pem \
    --from-file=consul-back-key.pem
```