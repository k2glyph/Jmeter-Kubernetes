

kubectl create -f rbac-config.yaml
helm init --service-account tiller --history-max 200

helm install --name jmeter stable/distributed-jmeter --set server.replicaCount=3

helm install --name influx stable/influxdb
## Go Inside influx db pod and create database

kubectl exec -i -t --namespace default $(kubectl get pods --namespace default -l app=influx-influxdb -o jsonpath='{.items[0].metadata.name}') /bin/sh
> create database jmeter;

helm install --name nginx stable/nginx-ingress


## Installing Cert-manager

kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.6/deploy/manifests/00-crds.yaml
kubectl create namespace cert-manager

kubectl label namespace cert-manager certmanager.k8s.io/disable-validation="true"

helm install --name cert-manager --namespace cert-manager stable/cert-manager

kubectl apply -f prod_issuer.yaml
kubectl apply -f staging_issuer.yaml


