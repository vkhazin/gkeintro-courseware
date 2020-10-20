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
1. Now we will port forward requests from Cloud Shell to a particular pod
1. Using the same terminal where you have executed `kubectl` commands run:
  ```
  kubectl port-forward web-0 8080:80
  ```
1. Open another tab in the Cloud Shell to send a request: `curl localhost:8080`
1. We should get `403 Forbidden` as we don't have `/usr/share/nginx/html/index.html` file yet
1. Using cloud shell let's create an `index.html` file in each pod:
```
for i in 0 1 2; \
  do kubectl exec "web-$i" -- sh -c 'echo "Hello NginX from $(hostname)!" > /usr/share/nginx/html/index.html'; \
done
```
1. Repeat the `curl` command a few times to see responses with different hostnames
1. Use `Ctrl-C` to stop the port forwarding in the first tab
1. Delete the first pod: `kubectl delete pod web-0`
1. Check with `kubectl get pods -w` to verify the pod has been re-created
1. Re-issue the port-forwarding command from before and in, a separate shell tab, the `curl` command
1. You should get the same hostname in the response as before even though the Pod has been deleted and re-created.
1. As the Pod's storage was preserved between Pod restarts