# k8s Quiz

What is this? Q&As generated from the Kubernetes documentaion.

Links

* https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
* https://kubernetes.io/docs/reference/access-authn-authz/rbac/

## What should be my thought process before draining a node?

1. Assess if applications running on node need to be highly available
2. Ensure applications that need PDBs have them

[Safely Drain Node](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/)

## What are the commands to safely remove a node from service? What does it mean to have a save eviction?

```
kubectl drain
```

> Safe evictions allow the pod's containers to gracefully terminate and will respect the PodDisruptionBudgets you have specified.

## Can I drain multiple nodes in parallel?

You can, but not with the same command. Do so in sepearate terminals or in the backround to ensure PDBs are respected.

> The kubectl drain command should only be issued to a single node at a time. However, you can run multiple kubectl drain commands for different nodes in parallel, in different terminals or in the background. Multiple drain commands running concurrently will still respect the PodDisruptionBudget you specify.

## Are there other options than `kubectl drain` if I need more fine grain control over the pod eviction process?

Sure, you can programatically cause evictions using the eviction API.

## What is RBAC? What is the API group in Kuberenetes?

RBAC is "Role-based access control" - a method of regulating access to computer or network resources based on the roles of individual users within your organization.

rbac.authorization.k8s.io

> RBAC authorization uses the rbac.authorization.k8s.io API group to drive authorization decisions, allowing you to dynamically configure policies through the Kubernetes API.

## How can I enable RBAC on Kubernetes?

> To enable RBAC, start the API server with the --authorization-mode flag set to a comma-separated list that includes RBAC; for example:

```
kube-apiserver --authorization-mode=Example,RBAC --other-options --more-options
```

## What are the four kinds of Kubernetes object?

Role, ClusterRole, RoleBinding and ClusterRoleBinding.

## With RBAC Roles & ClusterRoles, do deny rules exist?

Nope! purely additive

> An RBAC Role or ClusterRole contains rules that represent a set of permissions. Permissions are purely additive (there are no "deny" rules).

## True or False: When creating a RBAC Role, you have to specify the namespace it belongs in.

True

> A Role always sets permissions within a particular namespace; when you create a Role, you have to specify the namespace it belongs in.

## True or False: When creating a RBAC ClusterRole, you have to specify the namespace it belongs in.

False

> ClusterRole, by contrast, is a non-namespaced resource. The resources have different names (Role and ClusterRole) because a Kubernetes object always has to be either namespaced or not namespaced; it can't be both.

## ClusterRoles have several uses. Name the three I'm thinking of:

1. define permissions on namespaced resources and be granted access within individual namespace(s)
2. define permissions on namespaced resources and be granted access across all namespaces
3. define permissions on cluster-scoped resources

## What is the use case of Role vs ClusterRole

> If you want to define a role within a namespace, use a Role; if you want to define a role cluster-wide, use a ClusterRole.

## Define a role in the default namespace that can be used to grant read access to pods

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

## Since a ClusterRole can be used to grant the same permissions as a Role but are cluster-scoped, what are three use cases to grant access:

1. cluster-scoped resources (like nodes)
2. non-resource endpoints (like /healthz)
3. namespaced resources (like Pods), across all namespaces

## Would a Role or ClusterRole allow a particular user to run `kubectl get pods --all-namespaces`?

ClusterRole, because it's scoped to a cluster and not a namespace.


## Define a ClusterRole that can be used to grant read access to secrets in any particular namespace, or across all namespaces:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: secret-reader
rules:
- apiGroups: [""]
  #
  # at the HTTP level, the name of the resource for accessing Secret
  # objects is "secrets"
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```

## What can I use to grant permissions defined in a role to a user or set of users?

> A role binding grants the permissions defined in a role to a user or set of users

## What are the two components of a role binding?

* A list of subjects (users, groups, or service accounts)
* A reference to the role being granted

## Define a RoleBinding that grants a role `pod-reader` to the user `jane` in the `default` namspace.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
# This role binding allows "jane" to read pods in the "default" namespace.
# You need to already have a Role named "pod-reader" in that namespace.
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
# You can specify more than one "subject"
- kind: User
  name: jane # "name" is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  # "roleRef" specifies the binding to a Role / ClusterRole
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io
```

## Can a RoleBinding reference a ClusterRole to grant permissions defined in that ClusterRole to resources inside the RoleBinding's namepace?

Yes!

> This kind of reference lets you define a set of common roles across your cluster, then reuse them within multiple namespaces.

## Define a RoleBinding that grants a ClusterRole `secret-reader` to the user `dave` in the `development` namespace

```yaml
apiVersion: rbac.authorization.k8s.io/v1
# This role binding allows "dave" to read secrets in the "development" namespace.
# You need to already have a ClusterRole named "secret-reader".
kind: RoleBinding
metadata:
  name: read-secrets
  #
  # The namespace of the RoleBinding determines where the permissions are granted.
  # This only grants permissions within the "development" namespace.
  namespace: development
subjects:
- kind: User
  name: dave # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

## Define a ClusterRoleBinding that grants ClusterRole `secret-reader` to the group `manager`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
# This cluster role binding allows anyone in the "manager" group to read secrets in any namespace.
kind: ClusterRoleBinding
metadata:
  name: read-secrets-global
subjects:
- kind: Group
  name: manager # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

## After I create a binding, can I change the Role or ClusterRole that it refers to?

No

> After you create a binding, you cannot change the Role or ClusterRole that it refers to. If you try to change a binding's roleRef, you get a validation error.

> If you do want to change the roleRef for a binding, you need to remove the binding object and create a replacement.

## What is the logic behind making the Role or ClusterRole in a binding immutable?

1. Allows granting someone with `update` permission on a binding object the ability to manage the list of subjects (without chaning role)
2. A binding to a different role is a fundamentally different binding

> There are two reasons for this restriction:
> 1. Making roleRef immutable allows granting someone update permission on an existing binding object, so that they can manage the list of subjects, without being able to change the role that is granted to those subjects.
A binding to a different role is a fundamentally different binding. 
> 2. Requiring a binding to be deleted/recreated in order to change the roleRef ensures the full list of subjects in the binding is intended to be granted the new role (as opposed to enabling or accidentally modifying only the roleRef without verifying all of the existing subjects should be given the new role's permissions).

## What is the command line utility to create or update a manifest file containing RBAC objects - that handles deleting and recreating binding objects if required to change the role they refer to?

```
kubectl auth reconcile
```

## Explain a Kuberentes Subresource

A pod is a resource and it log is a resource. Example API
```
GET /api/v1/namespaces/{namespace}/pods/{name}/log
```

## How does RBAC refer to resources?

> RBAC refers to resources using exactly the same name that appears in the URL for the relevant API endpoint. Some Kubernetes APIs involve a subresource, such as the logs for a Pod. A request for a Pod's logs looks like: `GET /api/v1/namespaces/{namespace}/pods/{name}/log`


## Define a Role in the `default` namespace called `pod-and-pod-logs-reader` that gives `get` and `list` verbs to resource `pods` and subresource `logs`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-and-pod-logs-reader
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "list"]
```

## Define a Role in the `default` namespace called `configmap-updater` that gives `update` and `get` verbs to resource `configmaps` of names `my-configmap`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: configmap-updater
rules:
- apiGroups: [""]
  #
  # at the HTTP level, the name of the resource for accessing ConfigMap
  # objects is "configmaps"
  resources: ["configmaps"]
  resourceNames: ["my-configmap"]
  verbs: ["update", "get"]
```

## Why can I not restrict create or deletecollection requests by resource name?

> For create, this limitation is because the name of the new object may not be known at authorization time. If you restrict list or watch by resourceName, clients must include a metadata.name field selector in their list or watch request that matches the specified resourceName in order to be authorized. For example, kubectl get configmaps --field-selector=metadata.name=my-configmap

## What does it mean to aggregate ClusterRoles?

> You can aggregate several ClusterRoles into one combined ClusterRole. A controller, running as part of the cluster control plane, watches for ClusterRole objects with an aggregationRule set. The aggregationRule defines a label selector that the controller uses to match other ClusterRole objects that should be combined into the rules field of this one.

## Define an aggregated ClusterRole named `monitioring` that matches labels `rbac.example.com/aggregate-to-monitoring: "true"`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: monitoring
aggregationRule:
  clusterRoleSelectors:
  - matchLabels:
      rbac.example.com/aggregate-to-monitoring: "true"
rules: [] # The control plane automatically fills in the rules
```

## Since default role bindings authorize unauthenticated and authenticated users to read API information deemed safe to be publically accessible, how can I disable anonymous unauthenticaed access?

> To disable anonymous unauthenticated access, add `--anonymous-auth=false` to the API server configuration.

## How can I see permissions on the system:discovery clusterrole? How can I query for ClusterRoleBinds that use this ClusterRole as the `roleRef`?

Describe the ClusterRole:
```
kubectl get clusterroles system:discovery -o yaml
```

Get the ClusterRoleBindings:
```
k get clusterrolebinding -o=jsonpath="{.items[?(.roleRef.name == 'system:discovery')]}"
```
