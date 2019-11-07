# PAS for PKS on vSphere(PEZ)

Overview

## Description

## Demo

## Features

- feature:1
- feature:2

## Requirement

## Usage
### Certification for PKS API
```
$ echo <Enterprise PKS - PKS API - Certificate to secure the PKS API> > api.crt
```
### PKS Login
```
$ set -x HAAS <HAAS_NUMBER>
$ set -x UAA_ADMIN_PWD <Enterprise PKS - Credentials - Uaa Admin Password>
$ pks login -a api.pks.haas-$HAAS.pez.pivotal.io -u admin -p $UAA_ADMIN_PWD  --ca-cert api.crt
```

### PKS Cluster
```
$ pks create-cluster pas-for-kubernetes --external-hostname pas.haas-#HAAS.pez.pivotal.io --plan small
$ pks get-credentials pas-for-kubernetes
$ kubectl config get-contexts

CURRENT   NAME                 CLUSTER              AUTHINFO                               NAMESPACE
*         pas-for-kubernetes   pas-for-kubernetes   5ec25835-356e-4125-8fb0-b29230548aad
```

#### Hosts Configuration
```
$ pks cluster pas-for-kubernetes

PKS Version:              1.5.0-build.32
Name:                     pas-for-kubernetes
K8s Version:              1.14.5
Plan Name:                small
UUID:                     7fe91df2-8110-414c-b97f-f140ae2264ce
Last Action:              CREATE
Last Action State:        succeeded
Last Action Description:  Instance provisioning completed
Kubernetes Master Host:   pas.haas-XXX.pez.pivotal.io
Kubernetes Master Port:   8443
Worker Nodes:             3
Kubernetes Master IP(s):  10.123.456.789
Network Profile Name:
```

```
$ sudo vim /etc/hosts

10.123.456.789　pas.haas-XXX.pez.pivotal.io
```

### Namespace and Service Account
```
$ kubectl create namespace pas-system

namespace/pas-system created
```

```
$ kubectl create serviceaccount -n pas-system pas-system-service-account

serviceaccount/pas-system-service-account created
```

```
$ kubectl create clusterrolebinding pas-system-service-account-cluster-binding \
  --clusterrole=cluster-admin \
  --serviceaccount pas-system:pas-system-service-account

clusterrolebinding.rbac.authorization.k8s.io/pas-system-service-account-cluster-binding created
```

### PAS for Kubernetes
#### OCI Registry
- OCI Registory Hostname
  - <HARBOR HOSTNAME>
    - harbor.run.haas-###.pez.pivotal.io
- OCI Registry Credentials
  - admin
  - <PEZ MAIN PASSWORD>
- OCI Registry Repository Name Prefix
  - <REGISTRY_NAME>

#### Kubernetes
- Kubernetes URL
  - `Kubernetes master`
```
$ kubectl cluster-info

Kubernetes master is running at https://pas.haas-#.pez.pivotal.io:8443
CoreDNS is running at https://pas.haas-#.pez.pivotal.io:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

- Kubernetes Service Account
  - `pas-system-service-account`
```
secret_name="$(kubectl get serviceaccount -n pas-system \
  pas-system-service-account -o jsonpath='{.secrets[0].name}')"

kubectl get secret -n pas-system ${secret_name} -o jsonpath='{.data.token}' \
  | base64 --decode
```

- Kubernetes CA Certificate
```
kubectl config view --raw --minify \
  -o jsonpath='{.clusters[0].cluster.certificate-authority-data}' \
  | base64 --decode
```

### Harbor UI Login
- https://harbor.haas-###.pez.pivotal.io

### Harbor Login with Docker
- Certificate
  - Administration -> Configuration -> System Settings -> Registry Root Certificate
  - Download -> ca.crt

## Installation

## Licence

Released under the [MIT license](https://gist.githubusercontent.com/shinyay/56e54ee4c0e22db8211e05e70a63247e/raw/34c6fdd50d54aa8e23560c296424aeb61599aa71/LICENSE)

## Author

[shinyay](https://github.com/shinyay)
