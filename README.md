# CSC 519: Project

## Team Members

1) Kaushal Kheni (Unity ID: kkheni)

## Proposal

### Problem Statement



### Use case



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
