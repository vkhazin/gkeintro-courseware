# Cluster Security

* Limit SSH access to Kubernetes nodes
* Use `kubectl exec` for direct access to the container without access to the host
* Can use [Authorization Plugins](https://kubernetes.io/docs/reference/access-authn-authz/authorization/) to further restrict access
* Limit scope of user permissions with namespace to partition resources into named groups
* Define namespace resource quota to reduce risk of DoS or "noisy neighbor"
* GCP automatic firewall rules help preventing cross-cluster and cross-services communication
* Enable Kubernetes role-based access control (RBAC)
* Prefer a read-only access to the file system
* Log everything

[![](../media/k8s-cluster-security.webp)](https://www.giantswarm.io/blog/applying-best-practice-security-controls-to-a-kubernetes-cluster)
