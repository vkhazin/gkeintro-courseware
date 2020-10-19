# Lab: Stateful Set

1. Log in to Google Cloud Console
1. Navigate to Kubernetes Engine
1. Re-use the previously created cluster or create a new GKE cluster if none present
1. You can create a cluster using Web Console or using a Cloud Shell terminal:
```
export my_zone=us-central1-a
export my_cluster=standard-cluster-1
gcloud container clusters create $my_cluster \
   --num-nodes 3 --enable-ip-alias --zone $my_zone
```
1. Once the cluster is ready, select `Connect` link to initiate Cloud Shell to work with `kubectl`
1. Switch to a full screen mode and open an editor as well
1. Create a new file `stateful-set.yml` with the following content:
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: nginx
    labels:
      app: nginx
  spec:
    ports:
    - protocol: "TCP"
      port: 80
      targetPort: 80
    selector:
      app: nginx
    type: "LoadBalancer"

  apiVersion: apps/v1
  kind: StatefulSet
  metadata:
    name: web
  spec:
    selector:
      matchLabels:
        app: nginx
    serviceName: "nginx"
    replicas: 3
    template:
      metadata:
        labels:
          app: nginx # Pod template's label selector
      spec:
        terminationGracePeriodSeconds: 10
        containers:
        - name: nginx
          image: k8s.gcr.io/nginx-slim:0.8
          ports:
          - containerPort: 80
            name: web
          volumeMounts:
          - name: www
            mountPath: /usr/share/nginx/html
    volumeClaimTemplates:
    - metadata:
        name: www
      spec:
        accessModes: [ "ReadWriteOnce" ]
        resources:
          requests:
            storage: 1Gi
  ```
1. Apply the configuration: `kubectl apply --filename ./stateful-set.yml`
1. Check how many pods have been created: `kubectl get pods`
1. Depending on how fast the deployment processed you may see one, two, or all three pods running or some of them being `ContainerCreating`
1. Using Web Console navigate to `Kubernetes Engine` -> `Workloads` and explore `Persistent volume claims specification`
1. When you select the cluster you can navigate to `Storage` tab to see the storage provisioned
1. The `Storage` link will display persistent volume claims
1. We can also see the persistent volume claims using a command line: `kubectl get pvc`
1. Using the web console navigate to `Compute Engine` -> `Disks` to see the storage dynamically created
1. From the `Kubernetes Engine` -> `Services & Ingress` copy the combination of end-point and port for the load balancer, when the provisioning has completed
1. Using a terminal run the command: `curl http://loadbalancer-external-ip/`
1. We should get `403 Forbidden` as we don't have `/usr/share/nginx/html/index.html` file yet
1. Using cloud shell let's create an `index.html` file in each pod:
```
for i in 0 1 2; \
  do kubectl exec "web-$i" -- sh -c 'echo "Hello NginX from $(hostname)!" > /usr/share/nginx/html/index.html'; \
done
```
1. Repeat the terminal command: `curl http://loadbalancer-external-ip/` a couple of times to notice different hostname
1. You can also open a browser to point to the load-balancer IP, but you may not be getting different hostnames when refreshing