## Import pipeline from GitHub
1. On Harness "Continous Delivery & GitOps" section, click on the dropdown besides the "+ Create a Pipeline" button, and select "+ Import From Git"
2. Choose the "Third-party Git provider" option
3. On Git Connector, click on the selector and create a new connector.
4. Choose "GitHub Connector", and type in "Devops Harness ECS connector" as its name. Click on "Continue".
5. For "URL Type" choose "Repository", and "HTTP" for its "Connecetion Type"
6. Input the URL the your fork of this repository. Click on "Continue".
7. You may choose a different "Authentication" mode, but for here we will use "Username and Token"
8.  Type in your GitHub username
9. Click on "Create or Select a Secret" and a modal window will appear.
10. Click on "+ New Secret Text"
11. Type in a name for your secret.
12. For the secret value you will need to create a GitHub access token (classic) with the following permissions scope: "admin:repo_hoo", "repo", "user", see here for more information: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic
13. Click on "Save", and then on "Apply Selected"
14. Select "Enable API access", you may use the same access token. Click on "Continue".
15. Choose "Connect through Harness Platform". Click on "Save and Continue".
16. Click on "Finish" after the connection test is successful.
17. The "Repository" and "Git Branch" should autopopulate with the name of the repository and "main" respectively
18. For "YAML Path", the input should look like similar to this: `.harness/Pipeline_file.yaml`.

## Build Test and Run stage
Important: For this pipeline configuration you will need a Docker Hub access token with Read & Write permissions.

1. After importing the pipeline from the GitHub repository, navigate to the Build Test and Run stage and then to the "Install Node Modules" step.
2. Click on the "Optional Configuration" dropdown and then click on the "Container Registry" input to create a new Docker connector.
3. When prompted for "Connector type" choose "Docker Registry"
4. For its name type in "Nodejs DockerHub connector", it is important that the name is exactly this since the rest of the pipeline is already configured with it!
5. On the next screen leave the "DockerHub" option selected.
6. For the Docker Registry URL leave it as "https://index.docker.io/v2/"
7. Type in your docker hub username and create a new secret to store your access token
8. Choose Connect through Harness Platform and click on "Save and Continue"
9. In this same stage click on the "Build and push image to Docker Registry" step.
10. On the "Docker Repository" input type in the name of your public Docker Repository.
11. For the "Tags" input type in latest.
12. On the "Optional Configuration" dropdown, make sure "nodejsdockerfile" is in the "Dockerfile" input since in the "Create image" step we create a new Dockerfile.
13. Now access the "Build and Push to ECR" step, and the "aws connector" connector should be selected, if not click on the input a select it.
14. In the "Region" input type in "us-west-1".
15. In the "Account ID" you must type in your AWS account Id.
16. In image name make sure it is the same as the ECR repository previously created, in this case "devops-harness"
17. Type in "latest" for the "Tags" input
18. On the "Optional Configuration" dropdown, make sure "nodejsdockerfile" is in the "Dockerfile" input since in the "Create image" step we create a new Dockerfile.

## Integration test
19. Click on the "Integration Test" stage, and open the "Pull image and run it" step.
20. On the shell script, make sure to replace the docker image being pulled and ran on lines 9 and 11 to match your own.
21. Also, make sure the "Harness Docker Connector" is selected, and the "Image" input has "docker:latest" as value.