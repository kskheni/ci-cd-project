## Pre-requisite
* a GitHub self-hosted runner
* Docker installed on GitHub runner (needed for SuperLinter)
* Ansible installed on GitHub runner (To Configure Production environment)
* Repository secrets required in GitHub workflow
* Github runner should be able to SSH into production environment without needing a password

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
