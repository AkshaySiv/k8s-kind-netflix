
# Netflix Clone on kind Kubernetes Cluster

This project demonstrates how to deploy a simple **Netflix Clone** application on a local Kubernetes cluster using [`kind`](https://kind.sigs.k8s.io/).

## Prerequisites

Ensure the following tools are installed on your local machine:

- [Docker](https://www.docker.com/)
- [kind](https://kind.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- Netflix Clone source code or container image (e.g., hosted on DockerHub or local Dockerfile)

## Project Structure

```
netflix-clone/
├── app/                     # Netflix clone app source code
│   └── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
└── README.md
```

## Steps to Deploy

### 1. Create a kind Cluster

```bash
kind create cluster --name netflix-clone
```

### 2. Build the Docker Image

If you have the app locally:

```bash
cd app/
docker build -t netflix-clone:latest .
```

Load the image into the kind cluster:

```bash
kind load docker-image netflix-clone:latest --name netflix-clone
```

### 3. Deploy to kind

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

### 4. Access the Application

Expose the service locally (using port-forward):

```bash
kubectl port-forward svc/netflix-clone-service 8080:80
```

Visit your app at: [http://localhost:8080](http://localhost:8080)

## Kubernetes Manifests

### deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: netflix-clone
spec:
  replicas: 1
  selector:
    matchLabels:
      app: netflix-clone
  template:
    metadata:
      labels:
        app: netflix-clone
    spec:
      containers:
      - name: netflix-clone
        image: netflix-clone:latest
        ports:
        - containerPort: 80
```

### service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: netflix-clone-service
spec:
  selector:
    app: netflix-clone
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

## Cleanup

To delete the cluster:

```bash
kind delete cluster --name netflix-clone
```

## Diagram

Below is a high-level diagram of the local setup:

![Netflix Clone in kind Cluster](./A_diagram_in_the_image_illustrates_a_local_Kuberne.png)

## License

This project is licensed under the MIT License.
