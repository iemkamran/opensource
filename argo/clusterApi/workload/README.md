#Create a management cluster using Kind.
kind create cluster --config mgmt-cluster-config.yaml --name mgmt





#Initialize cluster-cli for Docker.
 clusterctl init --infrastructure docker

#Install ArgoCD
 kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

#Get Password for ArgoCD
 kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d