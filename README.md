# Voting Application

This is the voting application, a simple distributed application running across multiple Docker containers, which can be deployed using Docker Compose or Kubernetes.

## Getting Started

To get started with deploying the example voting application, follow the instructions below:

### Docker Compose

To deploy the application using Docker Compose, make sure you have Docker installed on your system.

- **For Mac or Windows**:
  Download [Docker Desktop](https://www.docker.com/products/docker-desktop) for Mac or Windows. [Docker Compose](https://docs.docker.com/compose) will be automatically installed.

- **For Linux**:
  Make sure you have the latest version of [Docker Compose](https://docs.docker.com/compose/install/) installed.

1. Clone this repository:

    ```bash
    git clone https://github.com/guddev99/votting-app.git
    ```

2. Navigate to the cloned directory:

    ```bash
    cd votting-app
    ```

3. Run the following command to start the services:

    ```bash
    docker-compose up
    ```

4. Access the voting interface at `http://localhost:5000` and the result interface at `http://localhost:5001`.

### Kubernetes

To deploy the application using Kubernetes, make sure you have `kubectl` installed and configured to connect to your Kubernetes cluster.

The folder k8s-specifications contains the yaml specifications of the Voting App's services.

1. Run the following command to create the deployments and services:

    ```bash
    kubectl create -f k8s-specifications/
    ```

2. Monitor the deployment until all services are up and running:

    ```bash
    kubectl get pods
    ```

3. Once all pods are in a `Running` state, you can access the Vote and Result interfaces using the respective services. This assumes that your Kubernetes environment supports LoadBalancer type services, such as GKE (Google Kubernetes Engine), EKS (Amazon Elastic Kubernetes Service), etc. Follow these steps:

    - Find the external IP address of the voting-service by running:
        ```bash
        kubectl get svc voting-service
        ```
        
        Then, navigate to the external IP address of the voting-service in your web browser to access the Vote interface.

    - Similarly, find the external IP address of the result-service by running:
    
        ```bash
        kubectl get svc result-service
        ```

        Then, navigate to the external IP address of the result-service in your web browser to access the Result interface.

    **Note:** If your Kubernetes environment does not support LoadBalancer type services, such as in on-premises or self-hosted environments, you may need to use alternative methods to access the services externally. This could include using NodePort type services, Ingress controllers, or other networking configurations depending on your specific environment setup.

## Architecture

![Architecture diagram](architecture.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/src/Worker) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) webapp which shows the results of the voting in real time

Note
----
The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.

This application is based on the [example-voting-app](https://github.com/dockersamples/example-voting-app) repository from the [dockersamples](https://github.com/dockersamples) GitHub page.
