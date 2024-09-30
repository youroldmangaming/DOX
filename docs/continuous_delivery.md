<img src="dox.png" width="150" height="100" alt="YOMG Lab Documentation">
# DOX - A Computer Scientists NoteBook

For full source visit [github](https://github.com/youroldmangaming/DOX/).

# Continuous Delivery and Continuous Deployment

Continuous Delivery (CD) and Continuous Deployment are advanced stages of the Software Development Lifecycle(SDLC) where automation is heavily used to ensure that code changes are seamlessly delivered to production environments. Though these terms are often used interchangeably, they have subtle differences:

## Continuous Delivery: 
The software is automatically built, tested, and made available for deployment at any time, but deployment to production still requires manual approval.

## Continuous Deployment: 
All change that passes the stages within the development pipeline are automatically deployed into production, this occurs without manual intervention.

The CD Pipeline Architecture is a structured process that involves a series of automated steps designed to move code from development through production, ensuring quality, reliability, and rapid release cycles.
In most modern software houses this is the goal independant of the scale of the organisation.

What I can say from experience is that if you start out with this as the basis when starting a software house then it is a lot easier than trying to instill these changes to people, processes and tools within a team who are already set in their ways.





### CD Pipeline Components:
#### Source Code Management (SCM) Integration:

Integrate with tools like GitHub, GitLab, or Bitbucket.
Whenever changes are pushed to specific branches (e.g., main, master), this triggers the CD pipeline.

Example: A git push triggers an automated job in Jenkins, GitLab CI, or GitHub Actions.
Build Automation:

Tools like Jenkins, GitLab CI, CircleCI, Travis CI, or GitHub Actions will pull the latest code from the repository and build it automatically.

Dependencies are installed, and the code is compiled into binaries or executables if needed.

Example: A Docker image is created or a compiled binary is produced from the code.
Automated Testing:

Unit tests, integration tests, and even performance and security tests are executed automatically.
Testing frameworks (e.g., JUnit, PyTest, Selenium) and tools for test automation (e.g., TestNG, Postman).
Example: After a successful build, unit tests ensure that individual components behave as expected.

####Artifact Repository:

Built artifacts are stored in repositories for later use. This allows the pipeline to deploy a consistent version of the code.
Tools: Nexus, Artifactory, Docker Hub (for Docker images).

Example: Docker images are pushed to a Docker registry, or JAR files are pushed to a Maven repository.
Environment Provisioning:

Automation tools like Terraform, Ansible, Kubernetes, or CloudFormation provision the required environments (development, staging, production) dynamically.

Example: A staging environment is created to deploy the built artifacts or containers for testing.
Deployment Automation:

Deployment to the target environment (e.g., staging, production) is automated using tools like Ansible, Jenkins, Kubernetes, AWS CodeDeploy, GitLab CI, or Azure Pipelines.

Example: Docker containers are deployed to a Kubernetes cluster, or the code is deployed to an AWS EC2 instance.
Automated Acceptance Tests:

Post-deployment tests are performed to verify that the application works as expected in the environment.

Example: Selenium tests ensure that the web interface functions correctly, while API tests ensure the backend is functional.
Monitoring & Feedback:

After deployment, real-time monitoring ensures that the application is running smoothly and gathers metrics on performance.
Tools: Prometheus, Grafana, Datadog, New Relic, ELK Stack.

Example: Application health is monitored to detect any issues after deployment, and feedback is provided to the development team.
Rollback Strategy:

In the case of failure or issues detected after deployment, automatic rollback mechanisms ensure that the previous working version of the software is reinstated.

Example: Kubernetes can automatically roll back to a previous working image if the current deployment fails.


## CD Pipeline Stages:

### Source Stage:

This stage monitors the source code repositories for changes.
Example: A commit to the main branch triggers the pipeline.

### Build Stage:

The code is compiled, and all dependencies are fetched.
Example: Maven, Gradle, or a Docker image build.

### Test Stage:

Unit tests, integration tests, and functional tests are executed.
Example: Selenium UI tests for a web app.

### Release Stage:

Artifacts are pushed to a repository, and the release is prepared.
Example: Docker images are uploaded to a Docker registry.

### Deploy Stage:

The built and tested application is deployed to staging, and subsequently, production.
Example: Helm charts are used to deploy a microservices app to Kubernetes.

### Monitor Stage:

Application performance and health are monitored post-deployment.
Example: Prometheus scrapes metrics, and Grafana displays the performance dashboard.
Tools Commonly Used in CD Pipelines:

### CI/CD Tools:

Jenkins, GitLab CI, GitHub Actions, CircleCI, Travis CI


### Artifact Management:

Docker Hub, Nexus, Artifactory

### Configuration Management:

Ansible, Chef, Puppet

### Container Orchestration:

Kubernetes, Docker Swarm

### Infrastructure as Code (IaC):

Terraform, AWS CloudFormation

### Monitoring:

Prometheus, Grafana, New Relic

### Continuous Delivery vs. Continuous Deployment:

#### Continuous Delivery:
After successful testing and quality checks, the deployment to production is manually triggered by the team.
Ideal when organizations need more control over the final push to production.

#### Continuous Deployment:
Every change that passes all tests is automatically deployed to production.
No manual approval is required, leading to faster release cycles and immediate updates to customers.

### Best Practices for CD Pipelines:
Automate everything: From testing to deployment, everything should be automated to ensure consistency and reliability.

Ensure rollback mechanisms: In case of failure, have robust rollback strategies.

Test early and often: Shift-left testing ensures that issues are caught early in the pipeline.

Monitor post-deployment: Continuous monitoring and feedback loops help identify issues in real-time.

This architecture streamlines the process of delivering high-quality software by automating most steps, reducing manual intervention, and ensuring quick feedback.

---
**DOX - A Computer Scientist's Notebook**  
_Y0MG 1990-2024_  
[GitHub Repository](https://github.com/youroldmangaming/DOX/tree/master) | [Documentation Site](https://dox.youroldmangaming.com)

---
