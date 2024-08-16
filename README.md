# Sock Shop Microservice Deployment (AltSchool Final Project)

## Project Overview
### Objective
Deploy a microservices-based architecture application (sock shop) on Kubernetes using a clear IaaC (Infrastructure as Code) deployment to be able to deploy the services in a fast manner.

- Socks Shop Microservices Demo: [GitHub Repository](https://github.com/microservices-demo/microservices-demo.github.io)
- Detailed Implementation Guide: [GitHub Repository](https://github.com/microservices-demo/microservices-demo/tree/master)

### Goal
This project is about deploying a microservices-based application using automated tools to ensure quick, reliable, and secure deployment on Kubernetes. By focusing on Infrastructure as Code, you'll create a reproducible and maintainable deployment process that leverages modern DevOps practices and tools.


## Task Instructions
### 1. Use Infrastructure as Code: 
Automate the deployment process. This means all the steps to get the application running on Kubernetes should be scripted and easily executable.

### 2. Focus on Clarity and Maintenance: 
Your deployment scripts and configurations should be easy to understand and maintain. Think of someone else (or yourself in the future) needing to update or replicate your setup.

### 3. Key Evaluation Criteria:

#### Deployment Pipeline: 
How the application moves from code to a running environment.

#### Monitoring and Alerts: 
Implement Prometheus for monitoring and set up Alertmanager for alerts.

#### Logging: 
Ensure the application's operations can be tracked and analyzed through logs.

#### Tools for Setup: 
Use either Ansible or Terraform for managing configurations. Choose an Infrastructure as a Service (IaaS) provider where your Kubernetes cluster will live.

### 4. Security and HTTPS: 
Make sure the application is accessible over HTTPS by using Let’s Encrypt for certificates. Consider implementing network security measures and use Ansible Vault for handling sensitive information securely.
## Extra Project Requirements for Bonus Points:
- **HTTPS Requirement:** The application must be securely accessible over HTTPS.
- **Infrastructure Security:** Enhance security by setting up network perimeter security rules.
- **Sensitive Information:** Use Ansible Vault to encrypt sensitive data, adding an extra layer of security.


## Deployment Steps

### STEP1: Installing Packages on Server

Create a medium RHEL instance on AWS (or if you have a local VM) for pipeline deployment. By running the installer.sh script, all necessary package and requirements needed to run the automation will be installed. The shell script is well documented to understand the process of :

1. **Updating the server** 
2. **Installing Terraform**
3. **Installing AWS CLI**
4. **Installing Kubectl**
5. **Installing Helm**
6. **Installing Jenkins and exposing port 8080**

```
 ./installer.sh
```
by running the above line on the shell, all necessary apckages are installed.


### STEP2: Setting Up Jenkins for Deploymeny

Now that all installation has been completed, Open jenkins in your browser:
    
    ``` 
    http://{RHEL_server_ip_address}:8080
    ```

Note, if you are running the RHEL on AWS, you'll need to configure the instance inbound rule of the associated security group to allow port **8080** access from a CIDR block or the whole internet.

Upon opening the URL, you'll be prompted for the inital admin password which can be gotten by running (you might need sudoers access):

    ```
    cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

Now, complete the setup by installing the suggested packages and plugins recommended by Jenkins aswell as creating a user.

Once all the above step has been done, you should be ready to dpeloy your application.

### STEP3: Running Infrastructure Pipeline on Jenkins

The first step is to provision necessary infrastructure that will be needed to run the applicaiton. We would provision the EKS cluster using the cluster-Jenkinsfile.

1. **Verify Plugins installation:**
Before moving ahead, plaese make sure that all required/suggested plugins by Jenkins have been installed. Such as the `Pipeline` plugin or aws credential plugin (if needed.)

2. **Setup Necessary Credentials and Environments:**
Set-up your AWS Credential in the Jenkins credential for use during the deployment process. Also, when setting up the pipeline, ensure to select build with parameters to setup an `Enviroment` parameters with choices of `create` or `delete`

3. **Configure the Pipeline Job:**
First, you configure a new jenkins items, using the `Pipeline` type. Since the code is hosted on a public github repo, we'll use select a SCM system  (e.g Git) for jenkin to retrieve our code from. 

Enter the Github URL and also the branch. Finally, choose the `cluster-Jenkins` as your pipeline script as it is our entry script to deploy the EKS cluster infrastructure to AWS and save your job settings.

4. **Build and Monitor:**
Now build the pipeline and monitor how the build is going. Upon building you'll be prompted if you'd like to create or destroy. Choose the option best preffered for you.

Overall, the Jenkins pipeline script (`cluster-Jenkinsfile`) automates the createion and destruction of an AWS EKS (Elastic Kubernetes Service) cluster using Terraform. Here's a breakdown of its functionality

### STEP4: Running Deployment Pipeline on Jenkins
The same steps carried out in step 3 should also be carried out in this step. The only difference is when configuring the pipeline, rather than choosing `cluster-Jenkins` as the pipeline script, we choose `Jenkinsfile` and continue on to build the job.

### Finally, you can view the deployed application on the hosted domain.