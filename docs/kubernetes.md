## Kubernetes

EKS version - 1.17

## Security
### [Pod Security Policies](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)
Pod Security Policies enable fine-grained authorization of pod creation and updates.
Control aspect / User case
- Running of privileged containers - `privileged`
- Restricting escalation to root privileges	- `allowPrivilegeEscalation` `defaultAllowPrivilegeEscalation`
- Usage of the host filesystem - `allowedHostPaths`
- Usage of volume types	- `volumes`
- Linux capabilities - `defaultAddCapabilities` `requiredDropCapabilities` `allowedCapabilities`
- The sysctl profile used by containers	- `forbiddenSysctls` `allowedUnsafeSysctls`
E.g.
```
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: privileged
  annotations:
    seccomp.security.alpha.kubernetes.io/allowedProfileNames: '*'
spec:
  privileged: true
  allowPrivilegeEscalation: true
  allowedCapabilities:
  - '*'
  volumes:
  - '*'
  hostNetwork: true
  hostPorts:
  - min: 0
    max: 65535
  hostIPC: true
  hostPID: true
  runAsUser:
    rule: 'RunAsAny'
  seLinux:
    rule: 'RunAsAny'
  supplementalGroups:
    rule: 'RunAsAny'
  fsGroup:
    rule: 'RunAsAny'
  ```

#### RBAC
RBAC is a standard Kubernetes authorization mode, and can easily be used to authorize use of policies.
First, a Role or ClusterRole needs to grant access to use the desired policies. The rules to grant access look like this:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: <role name>
rules:
- apiGroups: ['policy']
  resources: ['podsecuritypolicies']
  verbs:     ['use']
  resourceNames:
  - <list of policies to authorize>
```
Then the (Cluster)Role is bound to the ServiceAccount (recommended) or User (non-rocommended):
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: <binding name>
roleRef:
  kind: ClusterRole
  name: <role name>
  apiGroup: rbac.authorization.k8s.io
subjects:
# Authorize specific service accounts:
- kind: ServiceAccount
  name: <authorized service account name>
  namespace: <authorized pod namespace>
# Authorize specific users (not recommended):
- kind: User
  apiGroup: rbac.authorization.k8s.io
  name: <authorized user name>
```

 If a `RoleBinding` (not a `ClusterRoleBinding`) is used, it will only `grant usage for pods being run in the same namespace` as the binding. This can be paired with system groups to grant access to all pods run in the namespace. <br>
 To authorize all the `ServiceAccount` in a namespace:
```
# Authorize all service accounts in a namespace:
- kind: Group
  apiGroup: rbac.authorization.k8s.io
  name: system:serviceaccounts
# Or equivalently, all authenticated users in a namespace:
- kind: Group
  apiGroup: rbac.authorization.k8s.io
  name: system:authenticated
```

## [The 4C's of Cloud Native Security](https://kubernetes.io/docs/concepts/security/overview/)
![4c's of cloud security](images/4c.png)
### Cloud Security
#### Cloud provider security
- Identity and Access Control - Allow authorized access using IAM Roles and policies
- Network and Infrastructure Security - Protect your workloads from malicious or unauthorized traffic.
- Logging, Monitoring, Threat Detection, and Analytics - Centralized logging, reporting, and analysis of logs to provide visibility and security insights.
- Host and Endpoint Security
- Data Protection and Encryption
#### Infrastructure security
| Area of Concern for Kubernetes Infrastructure  | Recommendation
|---|---|---|---|---|
| Network access to API Server (Control plane)  | All access to the Kubernetes control plane is not allowed publicly on the internet and is controlled by network access control lists restricted to the set of IP addresses needed to administer the cluster.
|	Network access to Nodes (nodes)   | Nodes should be configured to only accept connections (via network access control lists)from the control plane on the specified ports, and accept connections for services in Kubernetes of type NodePort and LoadBalancer. If possible, these nodes should not be exposed on the public internet entirely.
| Kubernetes access to Cloud Provider API  | Each cloud provider needs to grant a different set of permissions to the Kubernetes control plane and nodes. It is best to provide the cluster with cloud provider access that follows the principle of least privilege for the resources it needs to administer. The Kops documentation provides information about IAM policies and roles.
| Access to etcd |Access to etcd (the datastore of Kubernetes) should be limited to the control plane only. Depending on your configuration, you should attempt to use etcd over TLS.
| etcd Encryption	| Wherever possible it's a good practice to encrypt all drives at rest, but since etcd holds the state of the entire cluster (including Secrets) its disk should especially be encrypted at rest.

### Container security
- Image Signing and Enforcement
- Container Vulnerability Scanning and OS Dependency Security
- Disallow privileged users inside containers

### Cluster security
There are two areas of concern for securing Kubernetes:
- Securing the cluster components that are configurable
- Securing the applications which run in the cluster
RBAC Authorization (Access to the Kubernetes API) - RBAC is a standard Kubernetes authorization mode, and can easily be used to authorize use of policies.
Authentication (Controlling Access to the Kubernetes API)
TLS For Kubernetes Ingress

### Code security
- Access over TLS only
- Limiting port ranges of communication
- Dynamic probing attacks (SQL injection, CSRF, and XSS. One of the most popular dynamic analysis tools is the OWASP Zed Attack proxy tool)

## Kubernetes Design Patterns
### Foundational patterns
Represent the principles and best practices that containerized applications
- Health Probe pattern - Define `probes` for `liveness` and `rediness` checks
- Predictable Demands pattern - Define `limit` and `requests` on `resources` 
- Automated Placement patterns - workload distribution in a multi-node cluster, Pod scheduling
### Structural patterns
- Init Container pattern - Init Containers enable separation of concerns by providing a separate life cycle for initialization-related tasks distinct from the main application containers
- Sidecar patterns - Describes how to extend and enhance the functionality of a pre-existing container without changing it (E.g. logging)
### Behavioral patterns
- Service Discovery pattern - How clients can access and discover the instances that are providing application services
- Stateful Service patterns - How to create and manage distributed stateful applications with Kubernetes
### Higher-level patterns
- Controller pattern - Controller is a pattern that actively monitors and maintains a set of Kubernetes resources in a desired state (Managed by EKS?)
- Operator pattern - Allows us to extend the Controller pattern for more flexibility and greater expressiveness.
  E.g. Argo CD (Continuous delivery tool for Kubernetes), Elastic Cloud on Kubernetes (Run Elasticsearch, Kibana, APM Server, Enterprise Search, and Beats on Kubernetes and OpenShift), Istio (Installs and maintain Istio service mesh)

## Assigning Pods to Nodes
Generally such constraints are unnecessary, as the scheduler will automatically do a reasonable placement (e.g. spread your pods across nodes, not place the pod on a node with insufficient free resources, etc.)
User cases:
- To ensure that a pod ends up on a machine with an SSD attached to it
- To co-locate pods from two different services that communicate a lot into the same availability zone.

### Taint and Tolerations
```
# Using `bootstrap.sh` in user-data
/etc/eks/bootstrap.sh $EKS_CLUSTER_NAME --kubelet-extra-args --register-with-taints=host=argus:NoSchedule
```

### Labels and Node selectors
```
# Using `kubectl label nodes`
kubectl label nodes <node-name> <label-key>=<label-value>
E.g. kubectl label nodes ip-172-30-17-140.ec2.internal mobileiron.io/worker=argus

# Using `bootstrap.sh` in user-data
/etc/eks/bootstrap.sh $EKS_CLUSTER_NAME --kubelet-extra-args --node-labels=mobileiron.io/worker=argus
```
Add selector to your Pod/Deployment/DaemonSet configuration
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  labels:
    app.kubernetes.io/instance: fluentd-forwarder-argusr
  name: fluentd-forwarder-argus
spec:
  containers:
  - name: fluentd-forwarder-argus
    image: 492608340914.dkr.ecr.us-east-1.amazonaws.com/fluentd-forwarder:1.0.0-5.release
    imagePullPolicy: IfNotPresent
  nodeSelector: 
    mobileiron.io/worker: argus

```
> **_NOTE:_**  The `NodeRestriction` admission plugin prevents kubelets from setting or modifying labels with a `node-restriction.kubernetes.io/` prefix. To make use of that label prefix for node isolation:

### Affinity and Anti-affinity
- The affinity/anti-affinity language is `more expressive`. The language offers more matching rules besides exact matches created with a logical AND operation
- You can indicate that the rule is `"soft"/"preference" rather than a hard requirement`, so if the scheduler can't satisfy it, the pod will still be scheduled
- You can constrain against labels on other pods running on the same node (or other topological domain), rather than against labels on the node itself, which allows rules
Categorized into two types:
#### Node affinity
Conceptually similar to nodeSelector -- it allows you to constrain which nodes your pod is eligible to be scheduled on, based on labels on the node.
- requiredDuringSchedulingIgnoredDuringExecution (hard)
   E.g. Only run the pod on nodes with Intel CPUs"

- preferredDuringSchedulingIgnoredDuringExecution (soft)
  E.g  Try to run this set of pods in failure zone XYZ, but if it's not possible, then allow some to run elsewhere

 > **_NOTE:_**  The `"IgnoredDuringExecution"` part of the names means that, similar to how nodeSelector works, if labels on a node change at runtime such that the affinity rules on a pod are no longer met, the pod will still continue to run on the node. In the future we plan to offer `requiredDuringSchedulingRequiredDuringExecution` which will be just like `requiredDuringSchedulingIgnoredDuringExecution` except that it will evict pods from nodes that cease to satisfy the pods' node affinity requirements.
```
apiVersion: v1
kind: Pod
metadata:
  name: with-node-affinity
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/e2e-az-name
            operator: In
            values:
            - e2e-az1
            - e2e-az2
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: another-node-label-key
            operator: In
            values:
            - another-node-label-value
  containers:
  - name: with-node-affinity
    image: k8s.gcr.io/pause:2.0
```

#### Inter-pod affinity and anti-affinity 
- Allow you to constrain your pod based on `labels on pods rather than based on labels on nodes`
- `label selector` over pod labels must specify which namespaces the selector should apply to
- The rules are of the form "this pod should (or, in the case of anti-affinity, should not) run in an X if that X is already running one or more pods that meet rule Y". `Y is expressed as a LabelSelector with an optional associated list of namespaces;` unlike nodes, because pods are namespaced (and therefore the labels on pods are implicitly namespaced), a label selector over pod labels must specify which namespaces the selector should apply to. Conceptually `X is a topology domain expressed using topologyKey like node, rack, cloud provider zone, cloud provider region etc`

 > **_NOTE:_** Note: Inter-pod affinity and anti-affinity require substantial amount of processing which can slow down scheduling in large clusters significantly. We do not recommend using them in clusters larger than several hundred nodes.

 > **_NOTE:_** Note: Pod anti-affinity requires nodes to be consistently labelled, in other words every node in the cluster must have an appropriate label matching topologyKey. If some or all nodes are missing the specified `topologyKey` label, it can lead to unintended behavior.

```
apiVersion: v1
kind: Pod
metadata:
  name: with-pod-affinity
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: security
            operator: In
            values:
            - S1
        topologyKey: failure-domain.beta.kubernetes.io/zone
    podAntiAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
            - key: security
              operator: In
              values:
              - S2
          topologyKey: failure-domain.beta.kubernetes.io/zone
  containers:
  - name: with-pod-affinity
    image: k8s.gcr.io/pause:2.0
```
- The affinity on this pod defines one `podAffinity` rule and one `podAntiAffinity` rule. 
- `topologyKey` cannot be empty
- The `podAffinity is requiredDuringSchedulingIgnoredDuringExecution` <br>
   It  says that the pod can be scheduled onto a node only if that node is in the same zone as at least one already-running pod that has a label with key `"security" and value "S1"`. (More precisely, the pod is eligible to run on node N if node N has a label with key `failure-domain.beta.kubernetes.io/zone` and some value V such that there is at least one node in the cluster with key `failure-domain.beta.kubernetes.io/zone` and value V that is running a pod that has a label with key "security" and value "S1".)

- The `podAntiAffinity is preferredDuringSchedulingIgnoredDuringExecution`.<br>
  It says that the pod cannot be scheduled onto a node if that node is in the same zone as a pod with label having key "security" and value "S2".

- 