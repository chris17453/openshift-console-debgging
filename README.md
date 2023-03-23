# openshift console debgging
Help, the openshift web cosole is down, and you cant login with oc login

## Problem, openshift web console app is down and oc login doesnt work

## What could it be?
- master nodes are down
- routes/gateway have changed
- DNS has been changed/lost on the bastion/kuberneties nodes
- cluster is overloaded and ejected the pod
- the ssl cert has expired?

## Gather some needed info
- The endpoint URL (BASEDOMAIN)
- DNS Servers on the kuberneties and bastion nodes
- Bastion Server Host IP/Credentials
- Master Node IP's
- Worker Node IP's
- What environement is this in? (lab/nonprod/dit/qa/uat/prod)
- did someone turn it off (think vcenter)


## First login to your cluster
- are you going through a jump box
```
oc login
```

# check the network
```
- traceroute to the masternodes to see if anything is blocked
```

## Check to see if the ip's are up
- for ech node
```
arp -a ip address
```
## GET ROUTES
```
# get the machine your on's routes
netstat -rn
#
# get the clusters routes
oc get routes
```

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
