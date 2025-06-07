k3d:
k3d cluster create local -a 3


ISTIO:
helm repo add istio https://istio-release.storage.googleapis.com/charts

helm install istio-base istio/base -n istio-system --wait

helm upgrade --install istiod istio/istiod -n istio-system --wait --cretae-namespace

kubectl label namespace <namesapce> istio-injection=enabled

kubectl get namespace -L istio-injection


Kiali:
helm repo add kiali https://kiali.org/helm-charts

helm upgrade --install kiali kiali/kiali-server -n kiali --create-namespace -f istio/kiali.yaml


Prometheus:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm upgrade --install prometheus prometheus-community/prometheus --version 27.20.0 -n prometheus -f prometheus/prometheus.yaml --create-namespace


curl:
kubectl run -it --rm -n test --image=curlimages/curl curly -- /bin/sh


Linkedir:
helm repo add linkerd-edge https://helm.linkerd.io/edge

helm install linkerd-crds linkerd-edge/linkerd-crds -n linkerd --create-namespace

helm install linkerd-control-plane \
  -n linkerd \
  --set-file identityTrustAnchorsPEM=ca.crt \
  --set-file identity.issuer.tls.crtPEM=issuer.crt \
  --set-file identity.issuer.tls.keyPEM=issuer.key \
  linkerd-edge/linkerd-control-plane

