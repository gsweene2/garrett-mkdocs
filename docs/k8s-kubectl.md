# k8s Kubectl

## Contexts

Get all contexts
```
k config get-contexts
```

Get current context
```
k config get-contexts -o name
```

Get current context
```
cat ~/.kube/config | grep current | sed -e "s/current-context: //"
```

## Pods

Getting started: what should I alias
```bash
alias k=kubernetes
```

List pods all namespaces
```bash
k get pods --all-namespaces
```

Count pods in all namespaces
```bash
k get pods --all-namespaces --output json | jq -j '.items | length'
```

List and sort all pods by age
```
k get pods -A --sort-by=.metadata.creationTimestamp
```

Create deployment with specific image (`nginx`)
```bash
kubectl create deployment $DEPLOY_NAME --image=$IMAGE
```

Get image(s) from deployment
```bash
k get deploy $DEPLOY_NAME -o=jsonpath={.spec.template.spec.containers[*].image}
````

Get image(s) from pod
```bash
k get pod $POD_NAME -o=jsonpath={.spec.containers[*].image}
```

List pods with name of nodes
```bash
k get pod -o=custom-columns=NAME:.metadata.name,STATUS:.status.phase,NODE:.spec.nodeName --all-namespaces
```

Get status of each container in pod
```bash
k get pod $POD_NAME -o=jsonpath='{range .status.containerStatuses[*]}{"\nImage: "}{.image}{"\nName: "}{.name}{"\nState: "}{.state}{"\n"}'
```

Create a pod from an image
```bash
k run $POD_NAME --image=$IMAGE
```

Preview yaml before creating pod
```bash
k run $POD_NAME --image=$IMAGE --dry-run=client -o yaml
```

## Namespaces

Get namespaces
```bash
k get ns
```

Count namespaces on system
```bash
k get ns -o json | jq '.items | length'
```

Count pods in a namespace
```bash
k get pods -n $NAMESPACE -o json | jq '.items | length'
```

Create a pod from image in a specific namespace
```bash
k run $POD_NAME --image=$IMAGE -n <ns>
```

## ReplicaSets

Get rs in a namespace
```bash
k get rs -n $NAMESPACE
```

What images are specified in replicaset?
```bash
k get rs $RS_NAME -o=jsonpath={.spec.template.spec.containers[*].image}
```

## Deployments

Get deployments in current namespace
```bash
k get deploy
```

Count deployments in current namespace
```bash
k get deploy --output json | jq -j '.items | length'
```

List all images for all deployments in current namespace
```bash
k get deploy -o=jsonpath={.items[*].spec.template.spec.containers[*].image}
```

Create deployment from image
```bash
k create deployment $DEPLOY_NAME --replicas=1 --image=some_image
```

## StatefulSet

Scale down
```bash
 k -n $NAMESPACE scale sts $STS_NAME --replicas 1 --record
 ```
## Services

Count services in the default namespace
```bash
k get svc -o json | jq '.items | length'
```

How can I describe a service?
```bash
k describe svc $SVC_NAME
```
