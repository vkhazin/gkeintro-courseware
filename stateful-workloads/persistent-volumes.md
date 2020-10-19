# Persistent Volumes

* Abstract storage provisioning from storage consumption
* Have a lifecycle independent of the Pod
* Makes it possible to avoid loss of data due to failures and rescheduling
* A cluster administrator can provision a variety of volume types
* Developers can use provisioned storage without creating and maintaining the volumes
* Volumes defined in Pods makes portability between Kubernetes platforms difficult, hence come the PVC

[![](../media/persistent-volumes.png)](https://portworx.com/tutorial-kubernetes-persistent-volumes/)