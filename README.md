# CI/CD Pipeline using Git, Jenkins, and Kubernetes

This project demonstrates a complete end-to-end CI/CD pipeline that automates the process of building, testing, and deploying applications using Git, Jenkins, and Kubernetes.

Whenever code is pushed to the Git repository, Jenkins automatically triggers a pipeline that builds the application, creates artifacts, and deploys them to a Kubernetes cluster. The pipeline is designed to be scalable, reusable, and production-ready with minimal manual intervention.

## Architecture Overview

The CI/CD pipeline follows a fully automated workflow integrating Git, Jenkins, Docker, and Kubernetes:

1. **Code Commit (Git)**
   - Developers push code changes to the Git repository.
   - A webhook is configured to notify Jenkins of new commits.

2. **Build Trigger (Jenkins)**
   - Jenkins automatically detects the commit and triggers the pipeline.
   - The pipeline is defined using a `Jenkinsfile` written in Groovy.

3. **Build & Artifact Creation**
   - Source code is checked out from the repository.
   - Application is built and packaged.
   - A Docker image is created and pushed to a container registry.

4. **Deployment (Kubernetes)**
   - Kubernetes manifests are applied using `kubectl`.
   - The new version of the application is deployed to the cluster.
   - Rolling updates ensure zero downtime deployment.

5. **Feedback & Logs**
   - Jenkins provides build status and logs.
   - Failures are reported with clear error messages.


## Architecture Diagram

<img width="840" height="439" alt="image" src="https://github.com/user-attachments/assets/b8d8355e-b553-41b7-bd6c-965d6c0f22d0" />

The diagram above represents the flow of the CI/CD pipeline:

Git Repository → Jenkins (Pipeline) → Docker (Image Build & Push) → Kubernetes (Deployment)

- Git triggers Jenkins via webhook
- Jenkins builds and pushes Docker image
- Kubernetes pulls the latest image and deploys it


## Tech Stack

The following tools and technologies are used to build and automate the CI/CD pipeline:

- **Version Control**
  - Git (for source code management)

- **CI/CD Tool**
  - Jenkins (pipeline automation using Groovy)

- **Containerization**
  - Docker (building and managing container images)

- **Container Orchestration**
  - Kubernetes (deployment and scaling of applications)

- **Scripting & Automation**
  - Groovy (Jenkins pipeline scripting)

- **CLI Tools**
  - kubectl (interacting with Kubernetes cluster)
  - docker CLI (building and pushing images)

- **Optional Enhancements**
  - Docker Hub / Private Registry (image storage)
  - Prometheus & Grafana (monitoring)


## Prerequisites

Ensure the following tools and configurations are in place before setting up the CI/CD pipeline:

### Required Tools
- Git installed and configured
- Jenkins installed and running
- Docker installed and running
- Kubernetes cluster (Minikube / Kind / Cloud-based)
- kubectl CLI configured to access the cluster

### Accounts & Access
- Access to a Git repository (GitHub / GitLab / Bitbucket)
- Docker Hub or a private container registry account
- Proper permissions to deploy resources in the Kubernetes cluster

### Jenkins Setup
- Required plugins installed:
  - Git Plugin
  - Pipeline Plugin
  - Docker Pipeline Plugin
  - Kubernetes CLI Plugin

### System Requirements
- Linux-based environment (recommended)
- Minimum 4GB RAM (8GB recommended for smooth Kubernetes operation)


## Project Structure

The repository is organized as follows:

## Project Structure

```bash
.
├── app/                    # application source code
├── k8s/
│   ├── deployment.yaml     # kubernetes deployment config
│   └── service.yaml        # kubernetes service config
├── Dockerfile              # container build definition
├── Jenkinsfile             # CI/CD pipeline definition
└── README.md               # project documentation
```


### Description

- **app/** → contains the main application code
- **k8s/** → holds kubernetes deployment and service files
- **Jenkinsfile** → defines the CI/CD pipeline stages
- **Dockerfile** → used to build the application image
- **requirements.txt** → lists project dependencies
  

## Implementation Steps

### Step 1: Git Integration and Automatic Trigger

- Integrated the Git repository with Jenkins using SCM configuration
- Configured a webhook in the Git repository to notify Jenkins on every commit
- Ensured that Jenkins automatically triggers the pipeline whenever code is pushed

This fulfills the requirement:
"Whenever a commit is made to a specific Git repository, Jenkins should automatically trigger a build process."


### Step 2: Build Process using Jenkins Pipeline

- Implemented a Jenkins Pipeline using a `Jenkinsfile` written in Groovy
- Defined pipeline stages to automate the build process
- Included stages such as:
  - Source Code Checkout
  - Application Build
  - Docker Image Creation
  - Docker Image Push to Registry (Docker Hub)

- Tagged the Docker image appropriately (e.g., latest / build number)
- Pushed the image to Docker Hub for use in Kubernetes deployment
- Ensured that the build process runs automatically after every successful trigger from Git

This fulfills the requirement:
"Upon commit, Jenkins should automatically trigger a build process."


### Step 3: Deployment to Kubernetes Cluster

- Configured Kubernetes manifests (`deployment.yaml` and `service.yaml`) for application deployment
- Updated the deployment configuration to use the latest Docker image from Docker Hub
- Added a deployment stage in the Jenkins pipeline to automate Kubernetes deployment
- Used `kubectl apply` to deploy and update resources in the cluster
- Ensured that the cluster pulls the latest image version during deployment
- Implemented rolling updates to ensure zero downtime

This fulfills the requirement:
"Upon successful build, Jenkins should deploy the artifact to a specified Kubernetes cluster."


### Step 4: Scalability and Adaptability

- Designed the pipeline to support multiple Git repositories by using parameterized configurations in Jenkins
- Ensured that the same pipeline structure can be reused across different projects with minimal changes
- Enabled Kubernetes deployments to different clusters/environments (e.g., dev, staging, production)
- Used modular and reusable pipeline stages in Groovy for flexibility
- Maintained separation of concerns between build, push, and deployment stages

This fulfills the requirement:
"The solution should be scalable and adaptable to accommodate different Git repositories and Kubernetes clusters."


### Step 5: Error Handling and Robustness

- Designed the Jenkins pipeline with clearly defined stages to isolate failures and improve traceability
- Configured the pipeline to fail fast, ensuring that errors in early stages (e.g., build) prevent unnecessary execution of later stages (e.g., deployment)

- Implemented detailed logging at each stage:
  - Build logs for compilation and dependency issues
  - Docker logs for image creation and push failures
  - Kubernetes logs for deployment and pod status

- Used proper exit codes in scripts to ensure Jenkins accurately detects success or failure
- Added retry mechanisms for transient issues such as:
  - Docker registry connectivity failures
  - Temporary Kubernetes API unavailability

- Ensured validation checks before deployment:
  - Verified successful image build and push before triggering deployment
  - Confirmed correct image tags are used in Kubernetes manifests

- Enabled visibility through Jenkins console output for quick debugging and monitoring

This fulfills the expectation:
"The solution should be robust, handling errors gracefully and providing clear feedback in case of failures."


### Step 6: Security Best Practices

- Secured sensitive information such as Docker Hub credentials and Kubernetes access using Jenkins Credentials Manager
- Avoided hardcoding secrets in the `Jenkinsfile` or source code

- Used role-based access control (RBAC) in Kubernetes to restrict access to cluster resources
- Ensured that only authorized users and services can deploy or modify workloads

- Configured secure communication between components:
  - Used HTTPS for Git and Jenkins communication
  - Secured access to container registry

- Applied image tagging strategy to avoid deploying unintended or unverified images
- Ensured only trusted images are pulled into the Kubernetes cluster

- Restricted Jenkins job permissions to prevent unauthorized pipeline execution or modifications

This fulfills the expectation:
"Security best practices should be implemented throughout the pipeline to safeguard code integrity and deployment environments."


## Jenkins Pipeline (Groovy)

The CI/CD pipeline is defined using a declarative `Jenkinsfile` written in Groovy. It automates the process of building, pushing, and deploying the application.

### Environment Variables

- `DOCKER_IMAGE` → Docker image name (`manojpatil1831/flask-app`)
- `TAG` → Image tag (`latest`)

These variables ensure consistency across build and deployment stages.

### Pipeline Stages

- **Checkout Code**
  - Clones the source code from the Git repository (`main` branch)
  - Uses Jenkins Git integration to fetch the latest code

- **Build Docker Image**
  - Builds a Docker image using the `Dockerfile`
  - Command used:
    ```bash
    docker build -t $DOCKER_IMAGE:$TAG .
    ```

- **Push to Docker Hub**
  - Uses Jenkins Credentials (`docker-creds`) for secure authentication
  - Logs into Docker Hub without exposing credentials
  - Pushes the built image to the registry
  - Logs out after push for security

- **Deploy to Kubernetes**
  - Applies Kubernetes manifests using `kubectl`
  - Commands used:
    ```bash
    kubectl apply -f k8s/deployment.yaml
    kubectl apply -f k8s/service.yaml
    ```
  - Deploys the latest Docker image to the cluster

### Key Highlights

- Fully automated pipeline from code checkout to deployment
- Secure credential handling using Jenkins Credentials Manager
- Uses shell commands (`sh`) for Docker and Kubernetes operations
- Simple and extensible structure for adding more stages (e.g., testing, monitoring)


## Kubernetes Deployment

The application is deployed to a Kubernetes cluster using manifest files located in the `k8s/` directory.

### Deployment Configuration

- `deployment.yaml` defines the application deployment
  - Specifies container image (`manojpatil1831/flask-app:latest`)
  - Configures replicas, labels, and container ports
  - Ensures the application runs in desired state

### Service Configuration

- `service.yaml` exposes the application
  - Defines how the application is accessed (ClusterIP / NodePort)
  - Maps external traffic to the application pods

### Deployment Process

- Jenkins executes the following commands during the deploy stage:
  ```bash
  kubectl apply -f k8s/deployment.yaml
  kubectl apply -f k8s/service.yaml


## Screenshots

### Jenkins Pipeline Execution

<img width="1469" height="701" alt="image" src="https://github.com/user-attachments/assets/840e5cea-827f-4b9e-b402-d5896ec42af1" />

All pipeline stages (Checkout → Build → Push → Deploy) completed successfully in Jenkins.

### Docker Image in Registry

<img width="1611" height="697" alt="image" src="https://github.com/user-attachments/assets/dec5171e-a18f-4793-8292-8038a33be7ca" />

Docker image successfully built and pushed to Docker Hub (`manojpatil1831/flask-app`).

### Application Output

<img width="1482" height="780" alt="image" src="https://github.com/user-attachments/assets/03381913-f665-4805-8fea-acc2618727e2" />

Application is successfully running and accessible after deployment.

### Kubernetes Pods Running

<img width="1338" height="678" alt="image" src="https://github.com/user-attachments/assets/68da35e1-42c7-4f57-910a-50d80fd69561" />

Pods verified in `Running` state using `kubectl get pods`.

## Conclusion

This project demonstrates the successful implementation of a fully automated CI/CD pipeline using Git, Jenkins, Docker, and Kubernetes, covering the complete software delivery lifecycle from code commit to production deployment.

The pipeline ensures that every code change pushed to the Git repository automatically triggers a Jenkins pipeline, which builds the application, containerizes it using Docker, and deploys it to a Kubernetes cluster. This automation significantly reduces manual intervention, minimizes human errors, and accelerates the overall development and release process.

By leveraging Docker, the application is packaged into a consistent and portable container, ensuring that it runs reliably across different environments. Kubernetes further enhances this setup by managing container orchestration, enabling features such as scaling, self-healing, and rolling updates for zero-downtime deployments.

Additionally, the pipeline is designed with scalability and flexibility in mind, allowing it to be reused across multiple projects and environments with minimal changes. Robust error handling, secure credential management, and modular pipeline stages contribute to a production-ready and maintainable solution.

Key highlights of this project include:
- End-to-end automation from code commit to deployment
- Integration of Jenkins pipelines using Groovy for flexible workflow management
- Reliable containerization and image management using Docker
- Efficient orchestration and deployment using Kubernetes
- Implementation of best practices for security, scalability, and fault tolerance

Overall, this project reflects a strong understanding of DevOps principles and modern CI/CD practices, providing a solid foundation for building scalable and efficient software delivery pipelines in real-world environments.






  

  





