# openfaas

https://github.com/openfaas/faas-netes/blob/master/chart/openfaas/README.md
tested on WRCP 21.05

# create namespaces
`kubectl apply -f https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml`

# add openfaas
`helm repo add openfaas https://openfaas.github.io/faas-netes/`

# update
```
helm repo update \
 && helm upgrade openfaas --install openfaas/openfaas \
    --namespace openfaas  \
    --set functionNamespace=openfaas-fn \
    --set generateBasicAuth=true
```

# check if it has deployed …repeat…
```
kubectl -n openfaas get deployments -l "release=openfaas, app=openfaas"
kubectl rollout status -n openfaas deploy/gateway
```

# get secret
```
PASSWORD=$(kubectl -n openfaas get secret basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode) && \
echo "OpenFaaS admin password: $PASSWORD"
```

# get port
```
kubectl get svc -n openfaas gateway-external -o wide
kubectl get services -A
```

# set OPENFAAS_URL on localhost
```
export OPENFAAS_URL=http://127.0.0.1:31112
```

# get and install faas-cli
```
curl -SLsf https://cli.openfaas.com | sudo sh
```

# faas-cli login and save credentials
```
echo -n $PASSWORD | faas-cli login -g $OPENFAAS_URL -u admin --password-stdin
```

# on remote host
`export OPENFAAS_URL=http://192.168.1.150:31112`
# same PASSWORD as before 
`echo -n $PASSWORD | faas-cli login -g $OPENFAAS_URL -u admin --password-stdin`

# remote browser
```
http://192.168.1.150:31112
user: admin
password: $PASSWORD
```