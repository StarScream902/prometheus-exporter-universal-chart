# Universal Helm Chart for the Prometheus exporters
---
## Examples

### Kafka exporter
```yaml
image:
  repository: "danielqsj/kafka-exporter"
  tag: "v1.4.2"
nameOverride: "kafka-exporter"
extraArgs:
  - --kafka.server=kafka:9092
```

### Mongodb exporter
```yaml
image:
  repository: "percona/mongodb_exporter"
  tag: "0.33"
nameOverride: "mongodb-exporter"
extraArgs:
  - --collect-all
  - --compatible-mode
secrets:
  MONGODB_URI: "mongodb://USERNAME:PASSWORD@mongodb/admin?replicaSet=rs0&ssl=false"
```

If you have a secret where you store your credentials, you may mount it in a Pod
### PostgreSQL exporter
```yaml
image:
  repository: "quay.io/prometheuscommunity/postgres-exporter"
  tag: "v0.11.1"
nameOverride: "postgres-exporter"
configs: # these directives will be created as env vars for an exporter
  DATA_SOURCE_URI: "postgresql-host:5432/postgres"
  DATA_SOURCE_USER_FILE: "/tmp/secrets/username"
  DATA_SOURCE_PASS_FILE: "/tmp/secrets/password"
volumeMounts:
  - name: secrets
    mountPath: "/tmp/secrets"
    readOnly: true
volumes:
  - name: secrets
    secret:
      secretName: some-secret-name
```
