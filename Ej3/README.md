# GitLab CI NGINX project

This pipeline is designed to run inside a NGINX GitLab repository in which the .gitlab-ci.yml file must be added.

## Considerations
- It is assumed before executing the pipeline that you have configured in GitLab runners of the docker container type and runners of the shell type, where these runners will have tags “docker” and “shell” respectively.
- It is assumed that you have a Docker image registry that both types of runners can access (in terms of network, firewalls, among others) and that the configured user and password can correctly access the registry. If not, configure them.
- It is assumed that you have configured the image registration and its requirements in such a way that within the jobs in the “image” field you will be able to download the image without any problems. If this is not the case, you must modify these fields in the 3 defined jobs.
- It is assumed that the index.html file is located in the www/index.html directory. In other words, inside the NGINX GitLab repository you should have the index.html file inside the www folder. If not, modify in the pipeline the path where the index.html file is located.

## Use Case
To use this pipeline in GitLab, certain variable values related to the particular execution environment must be configured:
- DOCKER_USER: user who can connect to the image registry.
- DOCKER_PASSWORD: password that can be connected to the image registry. It can be configured as a variable at the repository or group level in the CI/CD settings. This is done so as not to expose sensitive data as only people with high permissions on the GitLab repository can manage such variables. 
- TAG: define tag that will have the image to be built and push to the image registry.
- DOCKER_REGISTRY_URL: image registry url.
- INDEX_PATH: determines the directory where the index.html file is located within the GitLab NGINX repository.

Once these variables are set, you can add the .gitlab-ci.yml file to the GitLab repository and make a change to the index.html file and look at the Pipelines section within the NGINX repository.

## Pipeline description
This pipeline consists of three stages with one job each. 
- In the syntax check stage, the Hadolint tool is used to analyze the Dockerfile. It identifies syntax problems and bad practices in these files. A JSON file is generated with the results of the analysis of all Dockerfiles in a format compatible with GitLab Code Quality. This makes it possible to view a summary of the tests on the GitLab website.

- To start the job in the build stage, it waits for the successful completion of the syntax check stage job. The build job performs the login to the image registry and then builds the image.

- For the push stage job to start, it waits for the successful completion of the build stage job. The push job logs into the image registry, tags the image with the registry URL and the directory and name where the image will be stored. Finally it pushes the image.


## References
- [Hadolint: Complete Guide to Linting Dockerfiles](https://devopscube.com/lint-dockerfiles-using-hadolint/)