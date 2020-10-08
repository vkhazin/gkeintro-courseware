# Lab: GKE Logging & Monitoring

1. Navigate to [https://console.cloud.google.com/kubernetes/](https://console.cloud.google.com/kubernetes/workload)
1. Select the nodejs-endpoint workload we have created in the previous lab
1. You may want to hit the end-point using browser or curl command to generate some recent load and loads from the `Services & Ingress` navigation link
1. Notice the following:
   1. Container Logs
   1. Audit Logs
   1. CPU Load
   1. Memory Pressure
   1. Disk Utilization
   1. Time period selection
1. Proceed to `Services & Ingress` explore information available
1. Kubernetes comes with a Dashboard UI that is not deployed by default on GKE and is being [deprecated](https://cloud.google.com/kubernetes-engine/docs/concepts/dashboards) in favour of GKE Dashboards we've just explored
1. What can you gather from the GKE monitoring tools?
