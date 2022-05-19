Official Doc:

https://argo-cd.readthedocs.io/en/stable/getting_started/

# 1. Install Argo CD:
#This will create a new namespace, argocd, where Argo CD services and application resources will live.

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


#Create admin permission to argoCD that argocd used that permission and deploy project on k8s cluster 

kubectl create clusterrolebinding test-cluster-admin-binding --clusterrole=cluster-admin --user=swapnil@gmail.com

# 2. Access The Argo CD server as web-service
#Change the argocd-server service type to LoadBalancer

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

#or Port Forwarding 
#Kubectl port-forwarding can also be used to connect to the API server without exposing the service.

kubectl port-forward svc/argocd-server -n argocd 8080:443


# 3. Login Using The CLI
#initial username is admin and password store in plane text

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

#To change the default password go inside the pod

kubectl -n argocd exec -it argocd-server-6fc5956c76-9p5b2 -- bash

#change password using command 

argocd login <IP_ARGO_SERVER>   #get it from service
argocd account update-password  #will change login user password


# uninstall ArgoCD

kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml



