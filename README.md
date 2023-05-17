# operation
This repository contains all deployment and management files for the whole project for REMLA 2023.

# Local single deployment of the service stack
To deploy the service stack locally in a single deployment you can use the following docker compose as a start of:
```
services:
  backend:
    image: ghcr.io/remla23-team11/app:latest
    ports:
      - 5000

  frontend:
    image: ghcr.io/remla23-team11/model-service:latest
    ports:
      - 8080
    environment:
      - REACT_APP_API_URL=backend:8080
```

## Variables

* REACT_APP_API_URL — Used to set the backend URL that the frontend has to communicate with.

## Migration to Kubernetes
We have converted our .yml file from Docker compose to Kubernetes.

1. Start Minikube  
```
$ minikube start
```

2. Enable ingress
```
$ minikube addons enable ingress
```

3. open minikube dashboard (optional)
```
$ minikube dashboard
```

#### 4. Add Prometheus Repository. 
Run the following commands to install the Prometheus stack in your cluster.
```
$ helm repo add prom-repo https://prometheus-community.github.io/helm-charts
"prom-repo" has been added to your repositories

$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "prom-repo" chart repository
Update Complete. ⎈Happy Helming!⎈

$ helm repo list
NAME       URL
prom-repo  https://prometheus-community.github.io/helm-charts
```

#### 5. Install Prometheus Stack
The output contains an important information: the Prometheus instance is labeled with release=myprom.
```
$ helm install myprom prom-repo/kube-prometheus-stack
...
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace default get pods -l "release=myprom"
```

By default, Prometheus does not have an Ingress, so we need to use a port-forward again to access its website. We are interested in the **myprom-kube-prometheus-sta-prometheus** service that runs on port 9090. Run and the following command and keep the terminal open to see the dashboard of prometheus.
```
$ minikube service myprom-kube-prometheus-sta-prometheus --url
```

#### 6. Apply the configuration
Apply the file by the command:
```
$ kubectl apply -f /kubernetes/restaurant.yml
```
Once all **Pods** are in the state Running, run the command below to create a tunnel (keep the terminal open) for the Ingress and open **localhost/app** in your browser to access the frontend. 
```
$ minikube tunnel
```
At the user interface, for example, "We are glad we found this place." will be analyzed as a positive sentiment.
After pressing 'analyze', the feedback option will appear and after selecting the feedback, submit option will appear.
Related metrics will be monitored by prometheus and can be observed through the dashboard.
