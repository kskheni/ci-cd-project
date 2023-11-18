# Status Report 2

### Accomplishments:
- Created Ansible playbook for Nginx configuration, Docker installation, and other necessary tasks.
- Configured Nginx as a reverse proxy to forward traffic to coffee project
- Created Docker image to run ansible playbooks

Link to github commit: [this commits introduces ansible playbook used to configure the virtual machine](https://github.ncsu.edu/kkheni/CSC-519-Project/commit/ffb67bfdd66e8b1cb6e3c3dd59a4405b1db17648)

### Next steps:
- Develop GitHub Actions workflow to build the Docker image and push it to DockerHub after successful test cases on the main branch.
- Automate pull and deploy the latest Docker image on the remote server.
- adding Documentation for the project

### Retrospective:
- There were no issues in the development. However, I made a mistake in laying out plan for this sprint. While planning the tasks for this sprint, I did not consider the configuration phase and then later realized that it should be done before the deployment. So I had to make changes in the tasks of this sprint and work on configuration of remote machine instead of the deployment using docker image. 
