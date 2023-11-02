```
brew install minikube
brew install terraform
brew install kubectl

terraform init
terraform plan
terraform apply

kubectl expose deployment grafana --type=NodePort --port=3000 --namespace grafana
kubectl port-forward service/grafana 3333:3000  --namespace grafana

terraform destroy
```