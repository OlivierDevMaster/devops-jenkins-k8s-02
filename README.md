# Flask App

## Build the image

```bash
docker build -t flask_hello .
```

## Run the container

```bash
docker run -p 5000:5000 flask-app
```

or run test with

```bash
 docker run --rm --name flask_hello -p 5000:5000 flask_hello ./test.py --ve
```

# Configure kubernetes cloud in jenkins

Kubernetes url :

```bash
kubectl cluster-info
```

Jenkins url :

```bash
minikube service service-jenkins -n jenkins --url
```

Jenkins tunnel :

```bash

```
