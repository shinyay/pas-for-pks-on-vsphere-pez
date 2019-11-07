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
```
## Installation

## Licence

Released under the [MIT license](https://gist.githubusercontent.com/shinyay/56e54ee4c0e22db8211e05e70a63247e/raw/34c6fdd50d54aa8e23560c296424aeb61599aa71/LICENSE)

## Author

[shinyay](https://github.com/shinyay)
