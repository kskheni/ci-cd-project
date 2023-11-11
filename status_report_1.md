## Summary of work done till now:
Continuous integration including github actions and branch protection rules

## Report

### Accomplishments:
- a linter that is triggered on each push event and when a PR is opened or reopened on main, dev or release branches
- a testing workflow for node versions 16.x, 18.x and 20.x that is triggered on each push event and when a PR is opened or reopened on main, dev or release branches
- branch protection rules on main and release branch that mandate PR before merging, merge approvals and require status checks to pass before merging

Link to github commit: [this commits introduces changes that are needed to set working directory while installing coffee project dependencies and running tests](https://github.ncsu.edu/kkheni/CSC-519-Project/commit/6356342d358a978a8260eed9b481d30c0fca3075)

### Next steps:
- In the next phase, I plan to work on continuous delivery by building a docker image that encompasses all dependencies along with the coffee project and pushing it to dockerhub after testcases pass on main branch and then deploying it on the remote server to make the changes available to users.

### Retrospective
- So far, everything worked perfectly. However, I am thinking about making some changes to the testing part. Right now, testing does not happen using docker image that will be used in deployment. So I would like to discuss with instructors on how helpful it will be to run tests using the docker image that will be used while deployment.
