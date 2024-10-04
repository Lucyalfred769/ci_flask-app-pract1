# Simple Web Application Deployment with Kubernetes and CI/CD

This project demonstrates deploying a **Python Flask** web app on a **Kubernetes** cluster with a **CI/CD pipeline** using **GitHub Actions**. Basic security measures like containerization, non-root users and secrets management are implemented.

The **Simple Web App API** is a lightweight Flask application that provides users with a simple interface for interaction, featuring endpoints that respond with greetings and a welcome message. This application is designed to showcase essential web service functionality, making it an excellent starting point for learning about API development.


## Technologies Used
- **Flask** for the web app
- **Docker** for containerization
- **Kubernetes** for orchestration
- **GitHub Actions** for CI/CD
- **KinD** for local kubernetes testing

## Project Structure
```
/project-root 
├── app.py             # Flask application code
├── Dockerfile         # Docker configuration
├── requirements.txt   # Python dependencies
├── k8s/               # Kubernetes manifests
│ ├── Deployment.yaml  # Kubernetes deployment configuration
│ └── Service.yaml     # Kubernetes service configuration
└── .github/workflows/ # CI/CD pipeline configuration
    ├── ci.yaml
    └── cd.yaml        # GitHub Actions CI/CD script
```
## Getting Started
### Prerequisites
Before running the project locally, ensure you have the following installed:
- **Docker**, **KinD**, **kubectl**, **Git**
### Installation
1. **Clone the repository**: This repository contains all the files you need to deploy the Flask application. To get started, clone the repository:
    ```bash
    git clone https://github.com/mmumshad/simple-webapp-flask.git
    cd simple-webapp-flask
    ```

2. **Build the Docker image**:
    ```bash
    docker build -t simple-webapp:latest .
    ```

3. **Create a local Kubernetes cluster** with KinD:
    ```bash
    kind create cluster
    ```

4. **Apply the Kubernetes manifests** to deploy the app:
    ```bash
    kubectl apply -f k8s/
    ```

5. **Access the application**: Once the service is running, forward the port:
    ```bash
    kubectl port-forward svc/simple-webapp 8080:8080
    ```
   Visit `http://localhost:8080` to access the app.

## CI/CD Pipeline
This project uses **GitHub Actions** to automate the build, test, and deployment processes.
- **CI (Continuous Integration)**:
    - Each push or pull request triggers linting and validation of the Kubernetes manifests.
- **CD (Continuous Deployment)**:
    - Upon passing CI checks, the Docker image is built and pushed to a container registry.
    - The application is deployed to the Kubernetes cluster automatically.

### Pipeline Workflow:
1. **Build and test**:
    - The pipeline checks out the code, runs tests, and builds the Docker image.
2. **Push to registry**:
    - The image is pushed to Docker Hub (or another registry of your choice).
3. **Deploy to Kubernetes**:
    - The latest image is deployed to the Kubernetes cluster using the provided manifests.

You can find the complete CI/CD configuration in the `.github/workflows/ci.yaml` and `.github/workflows/cd.yaml` files, respectively.

## Kubernetes Deployment
The Kubernetes deployment uses two key manifest files:
- **Deployment**:
    - Defines the desired state of the application, including the container image and the number of replicas.
- **Service**:
    - Exposes the application to the external network via a `NodePort` service.

Steps to deploy:
1. **Deploy application**:
    ```bash
    kubectl apply -f Deployment.yaml
    ```

2. **Expose the service**:
    ```bash
    kubectl apply -f Service.yaml
    ```

3. **Port forwarding** for local access:
    ```bash
    kubectl port-forward svc/simple-webapp 8080:8080
    ```

## Security Measures
The following security best practices have been implemented:
1. **Containerization**:
    - The app is containerized using Docker to isolate dependencies and ensure consistency across environments.
2. **Least Privilege**:
    - The application runs as a non-root user inside the container, reducing the attack surface.
3. **Secrets Management**:
    - GitHub secrets were used for sensitive data handling. This ensures that credentials are not exposed in the source code. The secrets are securely accessed in the CI/CD pipeline when building and pushing Docker images.
4. **Automated CI/CD**:
    - GitHub Actions ensure that the deployment process is automated and consistent, reducing the risk of manual errors.

## Project Summary
- **Containerization**: The Flask app is containerized using Docker, with all dependencies bundled into a single image.
- **CI/CD Pipeline**:
    - GitHub Actions automate testing, building, and deploying the application to a Kubernetes cluster.
- **Kubernetes**:
    - Kubernetes is used to manage the application’s deployment, with manifests defining the desired application state and services to expose the app to external users.
- **Security**:
    - Basic security practices, such as running containers as non-root and managing secrets, have been integrated to safeguard the deployment.

## Conclusion
This project provides a basic yet secure Flask app deployment using kubernetes and GitHub Actions for CI/CD automation. It showcases how to efficiently manage the application's lifecycle while implementing essential security practices.

## Contributions & Suggestions
This project is open to collaboration! Whether you’re looking to suggest improvements, report issues, or contribute new features, your input is highly appreciated. Feel free to fork the repository, submit pull requests, or open issues.
