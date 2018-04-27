This repository contains a docker container and [Kubernetes] config for a service which proxies HTTP requests to [Google Cloud Storage][gcs] buckets. It is useful if you have a static website stored in a Google Cloud Storage bucket and want to serve it under a custom domain, possibly secured by a certificate.

# Quickstart

Create the service + deployment in your Kubernetes cluster (`kubectl apply -f service.yml`). Then create an [ingress] object to forward requests to the service.

The following ingress definition assumes you own a domain `cute.kittens`, have a `cute.kittens` Google Cloud Storage bucket with the static website, and use the [nginx ingress controller][nginx-ingress-controller].

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: cute-kittens
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
  - host: cute.kittens
    http:
      paths:
      - path: /
        backend:
          serviceName: proxy-to-storage
          servicePort: 80
```

# Notes

 - Internal traffic (between ingress and the service) is not encrypted.

[Kubernetes]: https://kubernetes.io/
[gcs]: https://cloud.google.com/storage/
[ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[nginx-ingress-controller]: https://github.com/kubernetes/ingress-nginx
