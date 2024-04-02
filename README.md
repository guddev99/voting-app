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
    kubectl get pods --watch
    ```

3. Once all pods are in a `Running` state, the `vote` web app is then available on port 31000 on each host of the cluster, the `result` web app is available on port 31001.

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
