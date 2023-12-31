# PreRequisites
 1. Make sure Docker Daemon is running
 2. HCP Account 
 3. HCP Service Principal with Viewer Permissions
 4. git
 5. kind
 6. kubectl

# Install Vault Secrets Operator

1. Create a Kubernetes cluster. 
~~~
kind create cluster --name vault-secrets-operator
~~~

2. Install the Vault Secrets Operator

~~~
helm repo add hashicorp https://helm.releases.hashicorp.com
helm repo update
helm install vault-secrets-operator hashicorp/vault-secrets-operator -n vault-secrets-operator-system --create-namespace
helm list -A
~~~

3. [Optional] Create a specific namespace to use

~~~
kubectl create namespace uber-api-dev
~~~

4. Create the Kubernetes Secret for the HCP Service Principal needed to fetch the secrets. For now, I've used Org Service Prinicipal with Viewer permissions. 

~~~
kubectl create secret generic vso-demo-sp --from-literal=clientID=$HCP_CLIENT_ID --from-literal=clientSecret=$HCP_CLIENT_SECRET
~~~

5. Configure VSO with your Org ID, Project ID & HCP Vault Secrets App Name to fetch secrets for your app 

~~~
kubectl apply -f hcp-auth.yaml
kubectl apply -f hvs-demo-app.yaml
~~~
