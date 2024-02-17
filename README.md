# WebSocket Scaling POC Project

## Overview

This Proof of Concept (POC) project demonstrates the scaling capabilities of a WebSocket server implemented in Golang, utilizing the Gorilla WebSocket library. The project includes containerization with Docker and deployment on Kubernetes with features like load balancing, preventing direct client communication, and integrating Redis Pub/Sub for efficient message broadcasting.

## Prerequisites

- Golang installed on your machine
- Docker installed
- Kubernetes cluster set up (Minikube for local development)
- kubectl configured to communicate with your cluster
- Redis server running (locally or in a cluster)

## Getting Started

1. Install Minikube:

    ```bash
    brew install minikube
    ```

2. Start Minikube:

    ```bash
    minikube start
    ```

3. Clone the repository:

    ```bash
    git clone https://github.com/manavendrasen/scalable-chat-app.git
    cd scalable-chat-app
    ```

4. Build the WebSocket server Docker image:

    ```bash
    docker build -t <your-user-name>/scalable-chat-app .
    ```

5. Push the docker image to a registry and modify the `k8.yaml` file to use your docker image (line 55)

	```bash
	docker push <your-user-name>/scalable-chat-app
	```

5. Deploy the WebSocket server and redis on Minikube:

    ```bash
    kubectl apply -f k8.yaml
    ```

6. Verify the deployment:

    ```bash
    kubectl get pods
    ```

## Testing

Get Minikube service url

	```bash
	minikube service chat-app-service
	```

Copy the url and open postman to connect to the websocket ws://<host>:<port>/ws

## Project Structure

- `server/`: Contains the Golang WebSocket server code.
- `Dockerfile`: Dockerfile for containerization.
- `k8.yaml`: Kubernetes deployment files.
- `README.md`: Documentation.

## Features

### 1. WebSocket Server Implementation

The WebSocket server is implemented using Golang and the Gorilla WebSocket library. Goroutines are used to handle concurrent connections efficiently.

### 2. Containerization

The WebSocket server is containerized using Docker, providing portability and ease of deployment.

### 3. Kubernetes Deployment

The WebSocket server is deployed on Minikube with three replicas and a LoadBalancer service, showcasing horizontal scaling and load balancing. Clients are isolated from direct communication.

### 4. Redis Pub/Sub

Redis Pub/Sub is integrated to enable broadcasting messages on a specific channel. This feature enhances real-time communication across WebSocket server instances.

### 5. Redis Integration in Kubernetes

Redis is added to the Minikube deployment to support the Pub/Sub functionality. The deployment configuration can be found in `k8.yaml`.

## Usage

- Connect to the WebSocket server using your preferred WebSocket client, specifying the LoadBalancer service IP.
- To send a message to all connected clients, publish a message to the specified Redis channel.

## Cleanup

To remove the deployed resources:

```bash
kubectl delete -f k8.yaml
minikube stop
minikube delete
``` 

## Conclusion

This project demonstrates the scalability and real-time communication capabilities of a WebSocket server in a containerized and Minikube Kubernetes environment, with the added benefit of Redis Pub/Sub for efficient message broadcasting. Feel free to explore and extend the project for your specific use cases.