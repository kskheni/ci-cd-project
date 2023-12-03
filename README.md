# CSC 519: Project

## Table of contents
* [Team Members](#team-members)
* [Project Proposal](#proposal)

<a name="team-members"></a>
## Team Members

1) Kaushal Kheni (Unity ID: kkheni)

<a name="proposal"></a>
## Project Proposal

### Problem Statement

The problem is the inefficiency in manually managing and deploying an application that grows over time with new requirements, dependencies and features. This process is prone to test failures. Traditional deployment practices result in critical issues like inconsistency of versions in development and deployment environments. Assume we want to deploy a large scale project with many dependencies and users, and we need to use a load balancer with 100 virtual machines, it will take plenty of time and resources to provision those virtual machines and set up them to run the software. Similarly, manually testing the application after each change is resource intensive and is prone to human error and unnoticed bugs which would become critical later in the development lifecycle. Manual operations limit the development team’s ability to release updates quickly and efficiently, and therefore, impacts both developers and end-users. Without a streamlined pipeline, developers struggle to ensure that the application is tested thoroughly and dependencies are consistent. As a result, the application may encounter frequent test failures, leading to delays in deployment and a lack of confidence in the deployed software's stability.

> ## Automation reduces the need for manual efforts

The pipeline designed by me packages the coffee project in docker image with all its dependencies, so that it can be easily deployed without worrying about dependencies and inconsistency of versions in development environment and deployment environment. It applies automated testing functionality that will run tests whenever new changes are committed or new Pull Requests have been created. Upon successful completion of tests, the PR can be merged with other branches. This frequent testing can help in finding critical bugs early in the development cycle and resolve them. 

Branch protection rules play an important role in huge teams, as it inhibits people’s power to make unwanted and irrevocable changes. Changes cannot be force pushed, deleted or merged into a protected branch until required status checks pass. This makes the code repository safe from intentional or unintentional harm from inside the team. When new changes are merged with the main branch, a new docker image that has project dependencies and source code is built and pushed to Docker Hub repository so that it could be used to release the new changes in the production environment. It also has release tags that could help in identifying versions of the project. The pipeline is designed in such a way that it can accommodate further changes in the coffee project. It is mostly event triggered, only approval to Pull requests needs human intervention. 

### Use case

```
Use Case: Deploy new changes that are made available on dockerhub image by github action workflow.
1 Preconditions
   Deployment machine provisioned.
   Self-Hosted GitHub Actions system provisioned.
   PR to main branch approved
   Github action for PR approval to main executed and new image pushed to dockerhub repository by the github action workflow
2 Main Flow
   DevOps engineer runs the ansible playbook to release new changes in the deployment environment[S1]. pulls new image from dockerhub [S2]. Create new container with same tag name using new image. [S3]
3 Subflows
  [S1] Checks if dependencies including Docker and Nginx installed on virtual machine. Install them if not already installed
  [S2] Pulls the image with new changes from docker hub repository. Check if an container with predetermined tag name is running. If it is running, remove it.
  [S3] Run coffee app inside new container
4 Alternative Flows
  [E1] Github workflow to push image failed.
```

### Pipeline Design

![alt text](https://github.ncsu.edu/kkheni/CSC-519-Project/blob/main/pipeline.png)

Assuming that the coffee project is ready to be deployed and released for the first time, we will first have to provision a virtual machine to host the project. The provisioning will be done manually as part of this project; however, some service providers also provide features to automate the provisioning process by using tools like Ansible. For example, AWS EC2. 

Following that, we will have to configure the virtual machine to install dependencies like Docker, Nginx and set up a reverse proxy that is required to host the project. We will use an ansible playbook to automate the configuration.

The software development will follow gitFlow and appropriate testing will be done after each commit and Pull Request using GitHub actions. Testing will include unit testing and linting. It may have other types of testing in an industry scenario e.g. Integration testing, System Testing. If tests fail after any commit, we can either notify team members, create an issue for the test failure or we can do both as well. 

Approving the Pull Request would require human Intervention. Once the Pull Request to the main branch has been tested and approved, a new docker image will be built and pushed on the dockerHub repository.

Now, we will check if all required dependencies for the virtual machine are already installed. We will use ansible to download the docker image on virtual machine from the docker hub repository and then create a container using this image that will run the coffee project.

In the gitflow, we will use semantic versioning for release branches to make it easier to compare versions and we will also tag main branch whenever 

**Re-release**

As we go further in the development process, the coffee project may grow and include more dependencies and we would like to automate as many tasks as possible while releasing those changes. To do that, we can check if any new dependency has to be installed, fetch the latest image from dockerhub, delete the currently running container and create a new docker container using the latest image.

During this whole process, Ansible playbooks have to be run manually.
