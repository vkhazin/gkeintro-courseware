# Lab: Kubernetes Security

1. Connect to previously created cluster using Cloud Shell
1. Create a new file `secret.yml` with the following content:
  ```
  apiVersion: v1
  kind: Secret
  metadata:
    name: test-secret
  data:
    username: bXktYXBw
    password: Mzk1MjgkdmRnN0pi
  ```
1. Use https://www.base64decode.org/ to decode the username and password values to see the clear text
1. Deploy the secret to the GKE cluster: `kubectl apply --filename ./secret.yml`
1. To get detailed information about the secret: `kubectl describe secret test-secret`
1. Create a new file `pod-with-secret.yml` with the following content:
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: pod-with-secret
  spec:
    containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        volumeMounts:
          # name must match the volume name below
          - name: secret-volume
            mountPath: /etc/secret-volume
    # The secret data is exposed to Containers in the Pod through a Volume.
    volumes:
      - name: secret-volume
        secret:
          secretName: test-secret
  ```
1. Deploy the Pod using the `kubectl apply ...` command
1. Get detailed information about the deployed pod: `kubectl describe...`
1. Verify the pod has access to the secret mapped as a volume `/etc/secret-volume`
1. Attache an interactive shell to the running pod `kubectl exec -i -t pod-with-secret -- /bin/bash`
1. Display the content of the mapped volume: `ls /etc/secret-volume`
1. Print clear text secrets:
```
echo "Username: $( cat /etc/secret-volume/username )"
echo "Password: $( cat /etc/secret-volume/password )"
```
1. Now let's define secrets via environment variables using the [online documentation](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/#define-container-environment-variables-using-secret-data)