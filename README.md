# Setting up Ingress on Kind


Create a cluster using the provided configuration file. For official documentation see https://kind.sigs.k8s.io/docs/user/ingress.

```sh
kind create cluster --config=config.yaml
```

Deploy the ingress controller on the cluster and wait for the deployment to be completed. This manifest was copied from https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml.
```sh
kubectl apply -f ingress-nginx.yaml
kubectl wait --namespace ingress-nginx   --for=condition=ready pod   --selector=app.kubernetes.io/component=controller   --timeout=90s
```

Deply an application the uses the ingress controller. This manifest was copied from https://kind.sigs.k8s.io/docs/user/ingress/#using-ingress and modified from apiVersion networking.k8s.io/v1beta1 to apiVersion networking.k8s.io/v1.
```sh
kubectl apply -f ingress-usage.yaml
```

Test the ingress
```
http localhost/foo
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 4
Content-Type: text/plain; charset=utf-8
Date: Fri, 30 Oct 2020 09:42:23 GMT
X-App-Name: http-echo
X-App-Version: 0.2.3

foo

http localhost/bar
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 4
Content-Type: text/plain; charset=utf-8
Date: Fri, 30 Oct 2020 09:42:57 GMT
X-App-Name: http-echo
X-App-Version: 0.2.3

bar
```
