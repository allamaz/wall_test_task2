# wallester_task_2: production-like Kubernetes deployment "my-k8s-deployment"




Objective

· Create a production-like Kubernetes deployment "my-k8s-deployment" which should expose a dummy 
Hello World page only to 88.196.208.91 IP address. It should have 2 containers and label 
"app:my-app".

● Container 1 should have the latest busybox image and should print current date inside every 1 
minute

● Container 2 may have your chosen image, resource requests and limits, readiness and liveness 
probes. 




Functional requirements:

·  production-like Kubernetes deployment

·  k8s 2 pods, each on different IP, each pod has 2 containers inside

·  self signed cert

·  https before ingress

·  http after ingress to pod

·  dummy web page container (browser access)

·  busybox container (printer)

·  ingress restriction for 1 single IP address access within browser

·  print record once per 1 minute

·  busybox latest (but fixed versions: busybox:1.37.0;)

·  other image (i got nginx alpine, fixed on nginx:1.29.3-alpine;)



Non-functional requirements:


· my-k8s-deployment.yaml

· Push the code to GitHub and share with Wallester for assessment

· production-like checklist:

     Security:
       + runAsNonRoot
       + readOnlyRootFilesystem
       + drop ALL capabilities
       + NetworkPolicy
       + TLS
       + Security headers
       + IP whitelist

     Reliability:
       + HPA
       + PodDisruptionBudget
       + Health probes startup, readiness, liveness
       + Pod anti-affinity
       + Resource limits incl. namespace

     Operations:
       + Rolling Update
       + Revision history rollback
       + Update process timeout
       + separate namespace



Evaluation:


Q: Are the requirements met?

A: Yes, it works as described in task.



### Tech.Stack

Implemented on Ubuntu Server v24.04.3 LTS with:
- k3s cluster v1.31.14+k3s1 (1 main + 2 workers)
- helm v4.0.1
- traefik v37.4.0
- cert-manager v1.19.1
- calico v3.29.0 (necessary for my needs on this cluster)
- rancher v2.13.0



### Deployment

```sh
kubectl create namespace myapp 2>/dev/null || true && kubectl apply -f my-k8s-deployment.yaml -n myapp && kubectl get hpa -n myapp -w
```

### Results


2 PODs created:

![Alt text](docs/k3s_pods.jpg?raw=true "example")



Each POD has 2 containers inside:

![Alt text](docs/k3s_each_pod_inide.jpg?raw=true "example")



Busybox is printing every minute (duplicates because we are using it in 2 containers):

![Alt text](docs/k3s_busybox.jpg?raw=true "example")



Self-signed cert is using:

![Alt text](docs/k3s_cert_selfsigned.jpg?raw=true "example")



HPA:

![Alt text](docs/k3s_hpa.jpg?raw=true "example")



Access in web UI from allowed IP address:

![Alt text](docs/k3s_web-app-https.jpg?raw=true "example")



Access in web UI with first random IP address:

![Alt text](docs/k3s_other_ip2.jpg?raw=true "example")



Access in web UI with second random IP address:

![Alt text](docs/k3s_other_ip1.jpg?raw=true "example")




