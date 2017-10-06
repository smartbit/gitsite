# Github Website deployed in GCE

This project deploys an website from github. When the git repository is updated, the site is automatically adapted.
It uses https://github.com/kubernetes/git-sync to pull the website from the repository. Use [git-sync image v.2.0.5 ](https://hub.docker.com/r/smartbit/git-sync/) or build your own [release](https://github.com/kubernetes/git-sync/releases).

# Steps to create
## Create clusters
If you don't have an Kubernetes cluster, create one and authenticate. Here we use the kuar-cluster of [Kubernetes: up and running](https://github.com/kubernetes-up-and-running)
```
gcloud container clusters create kuar-cluster
gcloud container clusters get-credentials
```
For details steps see Appendix.

## Deploy the webserver
```
kubectl apply -f secretlifeofpods.yaml
watch 'kubectl get services,deployments,replicasets,pods,namespace' # wait till the EXTERNAL-IP is provisioned
```
1. Open a browser session at http://EXTERNAL-IP.
1. Modify the [git repo](https://github.com/pieterlange/secretlifeofpods.git) and, after a refresh, see the changes appear in the browser.

# Tear down
```
kubectl delete -f secretlifeofpods.yaml
gcloud container clusters delete kuar-cluster # optionally kill the cluster to prevent those pesky bills
```

# ToDo
- Mutli-container deployment 
- Healthchecks & Readiness
- Logging (using the daemonSet `fluentd-gcp-v2.0`)
- Update and A/C-NAME record and use LetsEncrypt to get a certificate.

# Appendix
## Initialize
In a shell use these commands to install GCE and get an environment up and running:
```
curl https://sdk.cloud.google.com | bash
gcloud components install kubectl
gcloud init
gcloud auth login
gcloud container clusters create <cluster>
gcloud projects list
gcloud config set project <project>
gcloud auth application-default login
gcloud container clusters list
gcloud container clusters get-credentials <cluster>
```
