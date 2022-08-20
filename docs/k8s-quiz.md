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

# What is the use case of Role vs ClusterRole

> If you want to define a role within a namespace, use a Role; if you want to define a role cluster-wide, use a ClusterRole.

