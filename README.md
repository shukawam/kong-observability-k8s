# Kong Observability K8s

## How to use(Observability)

必要なネームスペースを作成します。

```sh
kubectl create ns observability
```

オブザーバビリティ基盤をデプロイするための Helm Chart を追加します。

```sh
# for kube-prometheus-stack
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
# for Grafana Loki/Tempo
helm repo add grafana https://grafana.github.io/helm-charts
# for OpenTelemetry Operator
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts

# update
helm repo update
```

オブザーバビリティ基盤をデプロイします。

```sh
# Prometheus + Grafana
helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack \
    -n observability \
    --values config/kube-prometheus-stack/values.yaml

# Grafana Loki
helm install loki-stack grafana/loki-stack \
    -n observability \
    --values config/loki/values.yaml

# Grafana Tempo
helm install tempo grafana/tempo \
    -n observability

# OpenTelemetry Operator
helm install opentelemetry-operator open-telemetry/opentelemetry-operator \
    -n observability \
    --values config/opentelemetry-operator/values.yaml

# OpenTelemetry Collector
kubectl apply -f config/opentelemetry-operator/collector.yaml -n observability

# OpenTelemetry Instrumentation
kubectl apply -f config/opentelemetry-operator/instrumentation.yaml -n observability
```

## How to use(Kong Gateway)

[Install Kong Gateway on-prem with Helm](https://developer.konghq.com/gateway/install/kubernetes/on-prem/) を参照の上、Control Plane / Data Plane を作成してください。その後、Data Plane を更新します。

```sh
helm upgrade kong-dp kong/kong --values config/kong/values-dp.yaml -n kong
```

## How to use(Demo Application)

アプリケーションをデプロイするためのネームスペースを作成します。

```sh
kubectl create ns services
```

アプリケーションをデプロイします。

```sh
kubectl apply -f demo-app.yaml
```
