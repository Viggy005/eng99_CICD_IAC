# eng99_CICD_IAC

- Jenkins to Create the CI/CD piple
    - a web hook to trigger the first job in jenkins
- Terraform to provision cloud resources(vpc,subnets,security groups,ec2 instances)
    1. VPC
    2. Subnets
    3. Security Groups
    4. ec2 instances launched in correct subnets
- ansible to configure the instances
    1. ec2 in public_subnet as internet facing app
    2. ec2 in private_subnet as mongodb Database
- JOB1 set up on Jenkins

## Step 1: set-up Jenkins on aws ec2 and Set-up JOB1
-       https://github.com/Viggy005/Jenkins_complete_automation
- follow steps from above link to set-up jenkins server
- follow along to create JOB1: web-hook, merge dev1 into main branch

## Step 2: set-up JOB2: terraform
- install terraform plugin
![](pics/jenkins_terraform_plugin.png)
![](pics/global_config.png)

- add aws credentials:
    - navigate to dashboard/configuration
        - find the heading "Global properties"
        - tick the box and write 3 env variables
        ![](pics/jenkins/env.png)


- jenkins job:
![](pics/jenkins/job.png)

    - run the job and infra will be created

# NOTE - need to set-up environment properly, main.tf needs correction.

# Day 4: set up ansible on jenkins to configure the ec2 instances provisioned on aws: 
## 3rd job to run ansible playbook/s to configure nodejs with reverse proxy on app instance

- https://plugins.jenkins.io/ansible/

### Step 1: SSH into jenkins Ec2
![](pics/jenkins-ansible/ansible-installed-ec2.png)

![](pics/jenkins-ansible/ansible-install-script.png)

### step 2: global tool config
![](pics/jenkins-ansible/global-tool-config.png)

### step 3: start jenkins pipeline job

- we will be generating declerative scrips to configure the pipline
![](pics/jenkins-ansible/click-declerative.png)
![](pics/jenkins-ansible/declerative-jenkins_script.png)


#### ERROR Along the way

![](pics/jenkins-ansible/job-error.png)

- we solve this by running the job once so that the working repo is available on the machine


### Step 4: run the job: build the job

![](pics/jenkins-ansible/result.png)

## Result:
- ec2 that was created by terraform is now configured to run NGINX by using ansible as a jenkins plug-in

# AMI of jenkins ec2 with terrafrom and ansible plug-in working:
-           ami-0c26d41557518450d



