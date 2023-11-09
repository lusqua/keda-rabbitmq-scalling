# AutoScalling based on RabbitMq queue size

## Description

This Project is a sample example of how to use Keda and RabbitMq to auto scale a service based on the queue size.

The project is composed by 3 services:

- RabbitMq: The message broker
- Builder: A service that produces messages to the queue
- Consumer: A service that consumes messages from the queue

I used the
[Keda](https://keda.sh/) to auto scale the consumer service based on the queue size,
[Helm](https://helm.sh/) to deploy the consumer service.

### Requirements

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Helm](https://helm.sh/docs/intro/install/)
- [Keda](https://keda.sh/docs/2.0/deploy/)
- [Docker](https://docs.docker.com/get-docker/)

## How to run

First, you need to start the minikube cluster:

```bash
minikube start
```

Then, you need to install the Keda:

```bash
helm repo add kedacore https://kedacore.github.io/charts
```

```bash
helm repo update
```

```bash
helm install keda kedacore/keda --namespace keda
```

After that, you need to install the RabbitMq:

```bash
kubectl apply -f rabbitmq/rabbitmq-deployment.yaml
```

```bash
kubectl apply -f rabbitmq/rabbitmq-service.yaml
```

Then, you need to install the consumer service:

```bash
helm upgrade --install golang-consumer helms/golang-consumer --namespace=default --wait
```

Finally, you can put messages in the queue using the port foward or usining the builder service:

```bash
kubectl port-forward svc/rabbitmq 5672:5672
```

### IN PROGRESS

```bash
helm upgrade --install golang-builder helms/golang-builder --namespace=default --wait
```
