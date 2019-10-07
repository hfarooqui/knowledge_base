## Kubernetes


- Kubernetes is an `open-source container management platform` which holds the responsibilities of container deployment, scaling & descaling of containers & load balancing
- Docker builds the containers and these containers communicate with each other via Kubernetes. Containers running on multiple hosts can be manually linked and orchestrated using Kubernetes
- Kubernetes is cloud-agnostic and can run on any public/private providers 
- Automated Scheduling
- Self healing
- Load balancing
- Rolling updates

<b>Metrics Server</b> is a cluster-wide aggregator of resource usage data. Resource metrics are used by components like kubectl top and the Horizontal Pod Autoscaler to scale workloads
<b>Prometheus Adapter</b> To autoscale based upon a custom metric

### Horizontal Pod Autoscaler (hpa)

- Automatically `scales the number of pods` in a replication controller, deployment or replica set based on `observed CPU utilization` (or, with `custom metrics` support, on some other application-provided metrics).  
- hpa does not apply to objects that can’t be scaled, for example, DaemonSets
- Implemented as a `Kubernetes API Resource` and a `Controller`
  - Resource determines the behavior of the controller
  - Controller periodically adjusts the number of replicas in a replication controller or deployment to match the observed average CPU utilization to the target specified by user
- hpa is implemented as a control loop, with a period controlled by the controller manager’s 
`--horizontal-pod-autoscaler-sync-period` flag (with a default value of 15 seconds)
- During each period, the `controller manager queries the resource utilization against the metrics specified in each HorizontalPodAutoscaler definition`. The controller manager obtains the metrics from either the `resource metrics API` (for per-pod resource metrics), or the `custom metrics API` (for all other metrics)
- E.g. `kubectl autoscale rs foo --min=2 --max=5 --cpu-percent=80`
- hpa does not work with rolling update using direct manipulation of replication controllers since hpa wont bind to new replication controllers. It only works with deployment object which in turn controls replica set
- `thrashing`- number of replicas keeps fluctuating frequently due to the dynamic nature of the metrics evaluated

### Rolling updates
The preferred way to create a `replicated applicatio`n is to use a `Deployment`, which in turn uses a `ReplicaSet`
 #### Update using confguration file
 ```
kubectl rolling-update my-nginx -f ./my-nginx.yaml

Created my-nginx-v4
Scaling up my-nginx-v4 from 0 to 5, scaling down my-nginx from 4 to 0 (keep 4 pods available, don't exceed 5 pods)
Scaling my-nginx-v4 up to 1
Scaling my-nginx down to 3
Scaling my-nginx-v4 up to 2
Scaling my-nginx down to 2
Scaling my-nginx-v4 up to 3
Scaling my-nginx down to 1
Scaling my-nginx-v4 up to 4
Scaling my-nginx down to 0
Scaling my-nginx-v4 up to 5
Update succeeded. Deleting old controller: my-nginx
replicationcontroller "my-nginx-v4" rolling updated
````
#### Update using spec property (container image)
```
# Update the pods of frontend-v1 to frontend-v2
kubectl rolling-update frontend-v1 frontend-v2 --image=image:v2

# Update the pods of frontend, keeping the replication controller name
kubectl rolling-update frontend --image=image:v2

```

If you encounter a problem, you can stop the rolling update midway and revert to the previous version using --rollback:
```
kubectl rolling-update my-nginx --rollback
```

### Taint and Tolerations

### Node selector
