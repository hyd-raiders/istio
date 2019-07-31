# origin

[origin readme](./README_origin.md)

# here start our tour

```bash
#install\kubernetes\helm\istio-init\values.yaml
hub & tag

#install\kubernetes\helm\istio\values.yaml

```





# install

```bash
# helm first

git clone -b dcjet https://github.com/hyd-raiders/istio.git

#helm service account
kubectl apply -f install/kubernetes/helm/helm-service-account.yaml
helm init --service-account tiller

# install istio-init
helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system

#check 54
kubectl get crds | grep 'istio.io\|certmanager.k8s.io' | wc -l

kubectl get pods -n istio-system


# install istio
helm install install/kubernetes/helm/istio --name istio --namespace istio-system

# check
kubectl get svc -n istio-system
kubectl get pods -n istio-system

#delet istio
# helm delete --purge istio
# helm delete --purge istio-init
kubectl get pods -n istio-system
```



# example

```bash
kubectl label namespace default istio-injection=enabled

kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
kubectl get services
kubectl get pods

kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
kubectl get gateway

kubectl get svc istio-ingressgateway -n istio-system

export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')

export INGRESS_HOST=$(kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')

export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT

curl -s http://${GATEWAY_URL}/productpage | grep -o "<title>.*</title>"
#<title>Simple Bookstore App</title>

kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml
kubectl get destinationrules -o yaml
```

