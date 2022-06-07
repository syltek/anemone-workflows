# Anemone Workflows

This is a collection of workflows for Anemone. We use them to build and deploy our services and libs.

## How to migrate from Jenkins

Migration to GitHub Actions is quite simple, you have to follow these steps:

1. Add to your project a .github/workflows folder.
2. Place in the folder the following files:

**commit.yml**
```yaml
name: 'Commit'

on:
  push:
    branches-ignore:
      - develop
      - main
      - master

jobs:
  build-and-verify:
    uses: syltek/anemone-workflows/.github/workflows/build.yml@main
    with:
      java_version: 17
    secrets:
      nexus_user: ${{ secrets.nexus_user }}
      nexus_password: ${{ secrets.nexus_password }}
```
***

**deployment.yml**
```yaml
name: 'Deploy'

on:
  push:
    branches:
      - develop
      - main
      - master

jobs:
  build-and-deploy:
    uses: syltek/anemone-workflows/.github/workflows/build-and-deploy.yml@main
    with:
      java_version: 17
      replicas: 6
    secrets:
      nexus_user: ${{ secrets.nexus_user }}
      nexus_password: ${{ secrets.nexus_password }}
      dockernexus_username: ${{ secrets.dockernexus_username }}
      dockernexus_token: ${{ secrets.dockernexus_token }}
      aws_docker_host_dev: ${{ secrets.aws_docker_host_dev }}
      aws_docker_host_pro: ${{ secrets.aws_docker_host_pro }}
```
***
**pull-request.yml**
```yaml
name: 'Pull Request'

on:
  pull_request:
    branches:
      - develop
      - main
      - master

jobs:
  pr-merge:
    uses: syltek/anemone-workflows/.github/workflows/build.yml@main
    with:
      java_version: 17
    secrets:
      nexus_user: ${{ secrets.nexus_user }}
      nexus_password: ${{ secrets.nexus_password }}
```
***
3. Adapt the files to your needs setting the java_version and replicas you wish according to your project needs.
4. Check your Jenkinsfile for any other configuration that might be missing. In that case, communicate it to the Backend Research Guild to address it.
5. If you did not find anything in step 4, delete the Jenkinsfile.
6. Test if your actions are running as expected opening a PR with the branch of your changes.
7. Ask the admin of the repository to remove the Jenkins condition from configuration.
