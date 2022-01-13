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

# Day 4: set up ansible on jenkins to configure the ec2 insatnces provisioned on aws: 
## 3rd job to run ansible playbook/s to configure nodejs with reverse proxy on app instance

- https://plugins.jenkins.io/ansible/