# openshift-console-debgging
openshift-cosole-debugging


## GET ROUTES
```oc get routes```

## CHECK INGRESS
```
oc -n openshift-ingress get pod -o json |
  jq -r '.items[].metadata.name' |
  xargs oc -n openshift-ingress delete pod
```  

## CHECK DNS
- does each Server use the same DNS Server?
  - if they are different then it can cause issues because entried might change over time

- test the dns entry
```
dig console-app.example.com
#
nslookup console-app.example.com
```

## CHECK CLUSTER CAPICITY
```
oc adm top node --heapster-namespace=openshift-infra --heapster-scheme=https
#
oc describe node
#
oc adm top
```


## CHECK CERT
```
oc get secret console-serving-cert -n openshift-console
#
oc delete secret console-serving-cert -n openshift-console
```
