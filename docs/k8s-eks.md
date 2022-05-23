# k8s EKS

## Install eksctl

[eksctl.io](https://eksctl.io/)

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv -v /tmp/eksctl /usr/local/bin
```

Check to see if it works
```
eksctl version
```

## Create a cluster

### With eksctl

Create the following yaml config as `k8s.yaml`:
```yaml
cat << EOF > k8s.yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: eksworkshop-eksctl
  region: ${AWS_REGION}
  version: "1.19"

availabilityZones: ["${AZS[0]}", "${AZS[1]}", "${AZS[2]}"]

managedNodeGroups:
- name: nodegroup
  desiredCapacity: 3
  instanceType: t3.small
  ssh:
    enableSsm: true

cloudWatch:
 clusterLogging:
   enableTypes: ["*"]

secretsEncryption:
  keyARN: ${MASTER_ARN}
EOF
```

Export
```bash
export MASTER_ARN=<iam_role>
export AWS_REGION=us-east-1
export AZS=(us-east-1a us-east-1b us-east-1c)
echo "Testing"
echo $MASTER_ARN
echo $AWS_REGION
echo ${AZS[0]}
```

Use eksctl to create cluster
```bash
eksctl create cluster -f k8s.yaml
```

Some Json Queries
```bash
# Get Stack Name
STACK_NAME=$(eksctl get nodegroup --cluster eksworkshop-eksctl -o json | jq -r '.[].StackName')

# Get Cluster IAM Role name
ROLE_NAME=$(aws cloudformation describe-stack-resources --stack-name $STACK_NAME | jq -r '.StackResources[] | select(.ResourceType=="AWS::IAM::Role") | .PhysicalResourceId')

# Export Role Name
echo "export ROLE_NAME=${ROLE_NAME}" | tee -a ~/.bash_profile
```

### Add Kubernetes Dashboard

Deploy Dashboard in cluster
```
export DASHBOARD_VERSION="v2.0.0"

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/${DASHBOARD_VERSION}/aio/deploy/recommended.yaml
```

Dashboard can be accessed at link
```
http://localhost:8080/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
```

Generate a token
```
aws eks get-token --cluster-name eksworkshop-eksctl | jq -r '.status.token'
```

#### Kill Dashboard

```
pkill -f 'kubectl proxy --port=8080'

kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/${DASHBOARD_VERSION}/aio/deploy/recommended.yaml

unset DASHBOARD_VERSION
```

### Clean Up Cluster

```
eksctl delete cluster -f k8s.yaml
```
