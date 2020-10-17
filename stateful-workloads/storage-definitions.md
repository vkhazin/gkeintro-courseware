# Storage Definitions

* PVC declaration:
  ```
  kind: PersistentVolumeClaim 
  metadata:
    name: my-volume-claim 
    spec:
      storageClassName: “standard" 
      accessModes:
        - ReadWriteOnce:
      resources:
        requests:
          storage: 100G
  ```
* PVC referenced from a pod:
  ```
  kind: Pod
  metadata: 
    name: demo-pod
    spec: 
      containers: 
        - name: demo-container
          image: "gcr.io/hello-app:1.0"
          PersistentVolumeClaim: 
            claimName: my-volume-claim
          volumeMounts: 
            - name: my-volume
              mountPath: /demo-pod
          volumes: 
            - name: my-volume
              PersistentVolumeClaim: 
                claimName: my-volume-claim
  ```
