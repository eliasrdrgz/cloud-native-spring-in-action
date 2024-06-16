# Cloud Native Spring in Action - adaptations

## Development environment

### knative cli

```bash
cd /tmp && \
curl -LO https://github.com/knative/client/releases/download/knative-v1.14.0/kn-linux-$(dpkg --print-architecture)
sudo install kn-linux-$(dpkg --print-architecture) /usr/local/bin/kn
```

```bash
kn service create \
quote-function \
--force \
--namespace dev \
--image docker.io/eliasrdrgz/quote-function:latest \
--request 'cpu=100m,memory=128Mi' \
--limit 'cpu=2,memory=512Mi' \
--annotation prometheus.io/scrape="true" \
--annotation prometheus.io/path=/actuator/prometheus \
--annotation prometheus.io/port="9102" \
--probe-liveness http::9102:/actuator/health/liveness \
--port 9102 \
--pull-policy 'IfNotPresent' \
--target=knative/quote-function-service.yml
```

### Kubernetes cluster

```bash
cat <<EOF | kind create cluster --name dev --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 31080 # expose port 31380 of the node to port 80 on the host, later to be use by kourier or contour ingress
    listenAddress: 127.0.0.1
    hostPort: 80
EOF
```

## Utilities


```bash
for i in `echo "catalog-service config-service dispatcher-service edge-service order-service quote-function quote-service"`; do
cd $i && gradle rewriteRun && cd -
done
```

## CHANGELOG

* Spring 3.3
* Java 21
* SBOM

