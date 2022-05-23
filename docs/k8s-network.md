# k8s Networking

## (0) Links

https://www.digitalocean.com/community/tutorials/how-to-inspect-kubernetes-networking
https://wiki.alpinelinux.org/wiki/Package_management
https://gist.github.com/BretFisher/5e1a0c7bcca4c735e716abf62afad389

## (1) Deploy an EKS Cluster with sample application

0. A k8s cluster with a service and a pod
1. Know the Pod name
2. Know the Service name

Write the pod name to POD_NAME
```
POD_NAME=upstream-nginx-7d948b789-gc9gl
```

## (2) Gather node, container, PID, & Kube DNS Info

```bash
# Identify the Node that the Pod is on
NODE_IP="$(k get pod $POD_NAME -o=jsonpath={.status.hostIP})"
# Identifiy the Docker container Id
CONTAINER_ID="$(k get pod $POD_NAME -o=jsonpath={.status.containerStatuses[0].containerID} | sed -e 's/docker:\/\///')"
# Identify the PID for the container
PID="$(docker inspect --format '{{ .State.Pid }}' $CONTAINER_ID)"
# Identify the kube-dns IP
KUBE_DNS_CIP="$(kubectl get service -n kube-system kube-dns -o=jsonpath={.spec.clusterIP})"
```

Verify everything looks okay (example output below)
```bash
echo "NODE_IP: $NODE_IP" &&
echo "CONTAINER_ID: $CONTAINER_ID" &&
echo "PID: $PID" &&
echo "KUBE_DNS_CIP: $KUBE_DNS_CIP"
```

Example Output:
```
NODE_IP: 192.168.65.4
CONTAINER_ID: 6d364b769e4493030072d143e7b10f5603902df609cc9d6377869d37c2ffc3d1
PID: 55554
KUBE_DNS_CIP: 10.96.0.10
```

## (3) Build the tools image & deploy a tools pod

The fillowing builds the image from Dockerfile in `k8s-tools KUBERNETES-RESOURCES/tools-image`. You can push to your Docker Hub or use the one I've pushed.

(Optional: Build and Push)
```bash
docker build -t k8s-tools KUBERNETES-RESOURCES/tools-image
docker image tag k8s-tools gsweene2/k8s-tools:0.2
docker push gsweene2/k8s-tools:0.2
```

Create a deployment with the tools image
```
k create deploy k8s-tools --replicas=1 --image=gsweene2/k8s-tools:0.2
```

## (4) Run a script to dump details
```bash
chmod +x ./KUBERNETES-RESOURCES/networking/inspect.sh

./KUBERNETES-RESOURCES/networking/inspect.sh \
    $TOOLS_CONTAINER \
    $CONTAINER_ID \
    $NODE_IP \
    $KUBE_DNS_CIP
```
