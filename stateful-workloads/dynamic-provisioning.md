# Dynamic Provisioning

* When Kubernetes cannot find an existing PersistentVolume, it will try to provision a new one
* Kubernetes, by default, dynamically provision a PersistentVolume
* When a matching PersistentVolume exists, Kubernetes will bind to it
* Omit `storageClassName` definitions and PersistentVolumeClaim will use the default StorageClass
* In GKE, the default StorageClass is named ‘standard’ 
* PersistentVolume can be retained when claim is delete with `persistentVolumeReclaimPolicy: Retain`

[![](../media/pv-static-dynamic.png)](https://www.bogotobogo.com/DevOps/Docker/Docker_Kubernetes_Persistent_Volumes_Dynamic_Volume_Provisioning.php)