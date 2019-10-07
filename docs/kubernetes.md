## Kubernetes

- Kubernetes is an `open-source container management platform` which holds the responsibilities of container deployment, scaling & descaling of containers & load balancing
- Docker builds the containers and these containers communicate with each other via Kubernetes. Containers running on multiple hosts can be manually linked and orchestrated using Kubernetes
- Kubernetes is cloud-agnostic and can run on any public/private providers 
- Automated Scheduling
- Self healing
- Load balancing
- Rolling updates

### Horizontal Pod Autoscaler (hpa)

- Automatically `scales the number of pods` in a replication controller, deployment or replica set based on `observed CPU utilization` (or, with `custom metrics` support, on some other application-provided metrics).  
- hpa does not apply to objects that can’t be scaled, for example, DaemonSets
- Implemented as a `Kubernetes API Resource` and a `Controller`
  - Resource determines the behavior of the controller
  - Controller periodically adjusts the number of replicas in a replication controller or deployment to match the observed average CPU utilization to the target specified by user
