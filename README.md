## Table of Contents:
* [Motivation](#motivation)
* [Description of Pipeline](#pipeline-description)
* [Prerequisites](#prerequisites)
* [Folder Structure](#folder-structure)
* [Setup steps](#setup)
* [Team members](team-members)

<a name="motivation"></a>
## Motivation/Automation reduces the need for manual efforts
The inefficiencies inherent in manual application management and deployment have been a persistent challenge in the software development process. The growth of our application, with its evolving requirements, dependencies, and feature sets, has highlighted the need for a streamlined Continuous Integration and Continuous Deployment (CI/CD) pipeline. This need arises from the inadequacies of traditional deployment practices, which often result in test failures, version inconsistencies between development and deployment environments, and significant resource expenditure.

The scenario becomes more complex when considering the deployment of a large-scale project with numerous dependencies and users, requiring the utilization of a load balancer across a large number of virtual machines. The manual provisioning and setup of these virtual machines become resource-intensive tasks, consuming both time and effort. Additionally, the likelihood of introducing inconsistencies in software versions between development and deployment environments is high, posing a significant risk to the stability and reliability of the application.

Furthermore, manual testing after each code change becomes a bottleneck, demanding extensive resources and leaving room for human error and undetected bugs. This approach is not only labor-intensive but also prone to overlooking critical issues that may manifest later in the development lifecycle. Manual operations not only limit the development team's ability to release updates promptly but also impact end-users by delaying the delivery of new features and improvements.

The presence of a streamlined CI/CD pipeline can help tackle these challenges, making it easier for developers to ensure thorough testing and consistent dependencies. Frequent test runs and bug detection help fixing issues earlier in the software development cycle, leading to additional confidence in deployment.

<a name="pipeline-description"></a>
## Description of pipeline:
![alt text](https://github.ncsu.edu/kkheni/CSC-519-Project/blob/dev/PROJECT_REPORT_PIPELINE.png)
To ensure the efficiency and reliability of the CI/CD pipeline, a GitHub runner has been set up with Docker and Ansible configurations. This runner is configured to SSH into the production environment without requiring password, laying a robust foundation for subsequent automated operations.

The CI/CD pipeline is designed to respond to key events in the development lifecycle. Linting and testing procedures are executed with every push event and pull request targeting designated branches (dev, main, release). This iterative validation process ensures the codebase's adherence to coding standards and maintains its reliability throughout its iterative evolution.

The project adheres to the GitFlow methodology and has branch protection rules for main and release branches. Pull requests to these branches necessitate the successful completion of status checks and approval from collaborators, ensuring a controlled and validated integration process. Release branches, created at the end of sprints or when the development of a few logical components has been completed, adhere to semantic versioning (major.minor.patch) and only allow bug fixes before the changes are finally deployed.

After resolving bugs in the release branch, a pull request to the main branch can be made. Approving this pull request triggers the deployment workflow. This workflow establishes connection with DockerHub using GitHub repository secrets for secure login. It builds a new image incorporating the latest coffee project codebase and pushes it to DockerHub. The execution of Ansible playbook guarantees the presence of all dependencies and establishes a reverse proxy for forwarding traffic to the coffee project. The automated deployment process involves stopping and removing the existing container, fetching the latest image from DockerHub, and deploying the coffee project with the updated image.

In this fully automated pipeline, human intervention is limited to the initiation and approval of pull requests. Use of GitHub actions enhances efficiency and markedly reduces the probability of errors during deployment.

Looking forward, as the coffee project matures and integrates additional dependencies, the automation scope will expand. The Dockerfile can be updated to automate the installation of all dependencies during the deployment workflow. In a similar way, the Ansible playbook may undergo changes to seamlessly accommodate evolving requirements. In case the coffee project demands use of load balancer with multiple servers in future, we can improve the deployment workflow and use ansible playbooks to easily deploy on multiple managed hosts. This forward-thinking approach ensures the continued adaptability of the CI/CD pipeline, capable of supporting the project's growth while maintaining a high degree of automation and efficiency.

<a name="prerequisites"></a>
## Prerequisites for the CI/CD pipeline to work efficiently
* a GitHub self-hosted runner
* Docker installed on GitHub runner (needed for SuperLinter)
* Ansible installed on GitHub runner (To Configure Production environment)
* Repository secrets required in GitHub workflow
* Github runner should be able to SSH into production environment without needing a password

<a name="folder-structure"></a>
## Folder Structure
```
CSC-519-Project
      |---- .github
      |         L---- workflows
      |                   |     deploy.yaml
      |                   |     nodeTest.yaml
      |                   L---- linter.yaml
      |---- Ansible
      |         |---- inventories
      |         |         L---- hosts.yaml    
      |         |---- nginx
      |         |         L---- nginx.conf
      |         |---- configure.yaml
      |         L---- Dockerfile
      |---- coffee-project
      |         |     // contains coffee project codebase
      |         |---- Dockerflie  
      L-------- L---- .dockerignore
      
```

### .github/workflows:
      This folder contains all the GitHub actions workflows. There are in total 3 workflows, deployment workflow, linting workflow and testing workflow.
<b>1) deploy workflow</b>
* Trigger: Triggered on the closure of pull requests to the main branch. 
* Steps: 
   * Builds and pushes the Docker image to Docker Hub. 
   * Uses Ansible to configure the production environment. 
   * Stops and removes the existing Docker container, pulls the new image, and starts a new container. 

<b>2) linter workflow</b>
* Trigger: Triggered on every push and pull request to specified branches. 
* Steps: 
   * Checks out the code and uses Super-Linter to perform linting for Node.js, JavaScript, Ansible, HTML, and JSON. 

<b>3) testing workflow</b>
* Trigger: Triggered on push and pull requests to designated branches.
* Steps: 
   * Checks out the code, sets up Node.js environment, installs dependencies, and runs automated tests. 

### Ansible:
      This folder has files needed to configure the production environment. It will be used when deploy workflow triggers and will configure the production environment without manual effort. It also has nginx.conf file that is used inside workflow to set up reverse proxy. It also has a Dockerfile that could be used to run ansible playbook inside a container.
* Installs Nginx.
* Manages Docker setup, removing conflicts.
* Set up reverse proxy to forward traffic to the coffee project running inside a container.

### coffee-project:
      This folder contains the sourcecode of coffee project that is made using NodeJS. It can be containerized using the Dockerfile.

<a name="setup"></a>
## Setup steps:
- Since this is an automation pipeline, there is not much that you have to do on the local environment. You need to have an github runner that has docker to pull and run Superlinter and ansible installed for configuring the production environment.
- There is a set of secrets that are being used in github workflows, so you need to store them in repository secrets.
- GitHub runner should be able to SSH into production server without password. For this, you will have to authorize SSH key of runner in production server.

Once this steps are completed, the CI/CD pipeline will work smoothly and efficiently.

### local setup steps:

```
npm install
node app.js
```

Testing:
```
npm install --include=dev
npm test
```

<a name="team-members"></a>
## Team Members

1) Kaushal Kheni (Unity ID: kkheni)
