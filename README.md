k3d:
k3d cluster create local -a 3

Kubernetes Dashboard:


ISTIO:
helm repo add istio https://istio-release.storage.googleapis.com/charts

helm upgrade --install istio-base istio/base -n istio-system --wait

helm upgrade --install istiod istio/istiod -n istio-system --wait --cretae-namespace 

kubectl label namespace <namesapce> istio-injection=enabled

kubectl get namespace -L istio-injection


Kiali:
helm repo add kiali https://kiali.org/helm-charts

helm upgrade --install kiali kiali/kiali-server -n kiali --create-namespace -f istio/kiali.yaml


Prometheus:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm upgrade --install prometheus prometheus-community/prometheus -n prometheus -f prometheus/prometheus.yaml --create-namespace

Grafana:
helm upgrade --install grafana grafana/grafana -n grafana --create-namespace -f grafana/grafana.yaml


curl:
kubectl run -it --rm -n test --image=curlimages/curl curly -- /bin/sh

kubectl exec curl-loop-86bff9bfbc-5s6x7 -n test -c curl -- curl http://sample-app-service.test


Linkedir:
helm repo add linkerd-edge https://helm.linkerd.io/edge

helm upgrade --install linkerd-crds linkerd-edge/linkerd-crds -n linkerd --create-namespace

helm upgrade --install linkerd-control-plane \
  -n linkerd --create-namespace \
  --set-file identityTrustAnchorsPEM=ca.crt \
  --set-file identity.issuer.tls.crtPEM=issuer.crt \
  --set-file identity.issuer.tls.keyPEM=issuer.key \
  linkerd-edge/linkerd-control-plane \
  -f linkerd.yaml

linkerd check

linkerd viz install | kubectl apply -f -

linkerd viz dashboard
