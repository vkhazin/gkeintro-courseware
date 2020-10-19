# Kubernetes Secrets

* Secrets let store and manage sensitive information, such as passwords, oAuth tokens and etc
* Secret is an object that contains a small amount of sensitive data
* Can be used with a Pod in three ways:
  1. As container environment variables
  1. Mounted as files from a Pod
  1. By the cluster to authenticate to a private image repository
* Secrets are scoped to a namespace, base64 encoded and can be easily decoded to the original value:
```
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  password: cXVpY2suZm94LjUzMTI=
  user: ZGJ1c2Vy
```
* A better approach is integration with GCP Secrets Manager - work in progress at the moment