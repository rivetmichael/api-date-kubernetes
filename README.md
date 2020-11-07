# How to use ?

## Install Kubernetes cluster locally with Minikube

```
# Start local cluster
minikube start --cpus 2 --memory 8192 -p reezocar
minikube -p reezocar addons enable ingress
```

```
# Create dedicated namespace
kubectl create ns reezocar
kubectl config set-context --current --namespace=reezocar
```

## Building the API project

The API is a PHP Symfony app, the repository is here https://github.com/rivetmichael/api-date.
Builds are automatically triggered when a tag is created and push to Docker hub : https://hub.docker.com/repository/docker/mrivet/api-date


## Deploy with Helm

The API can be deployed with Helm template 

```
# Deploy the with default values
helm install api ./api-date

# If you want to update
helm upgrade api ./api-date --wait
```

## Access it 

```
export API_IP=$(minikube -p reezocar ip)
export API_URL=api.reezocar.local
curl -H "Host: $API_URL" http://$API_IP/getDate
```

## Deploy another instance with different timezone

```
# Deploy with values file for GMT timezone
helm install api-gmt ./api-date -f kubernetes/values.GMT.yaml

# Access it
export API_URL=api.gmt.reezocar.local
curl -H "Host: $API_URL" http://$API_IP/getDate
```

## Delete local cluster

```
minikube -p reezocar delete
```
