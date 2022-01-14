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
- dont forget to edit the playbook(.yml) and inventory(.inv) files on github to do perform desired tasks on specific instances 

![](pics/jenkins-ansible/result.png)

## Result:
- ec2 that was created by terraform is now configured to run NGINX by using ansible as a jenkins plug-in
- usefull link: https://www.youtube.com/watch?v=PRpEbFZi7nI&t=329s

# AMI of jenkins ec2 with terrafrom and ansible plug-in working:
-           ami-0c26d41557518450d

# day 5 (make app run with posts, figureout how to get IP from terraform job to Ansible Inventory file)
## set-up app:
- ssh into jenkins ec2
- cd /etc/ansible
- create a provision script (sudo nano provsision.sh)
    ![](pics/jenkins-ansible/provision-app-script.png)

- create a file called default at location /etc/ansible on the jenkins ec2
    ![](pics/jenkins-ansible/default.png)

- edit playbook on git hub:
    ![](pics/jenkins-ansible/playbook-appsetup.png)

- run the job on jenkins
- ssh into app ec2
- npm install & npm start

## set-up database
- launch database in public subnet
so we can run ansible playbook from jenkins(not best practice)
- create a file on git-hub called setup_db.yml

- create a mongodb.conf file in jenkins ec2 at location /etc/ansible
    ![](pics/jenkins-ansible/mongodb-conf-jenkins-ec2.png)
- create a provision_update.sh file at location /etc/ansible on jnekins ec2
    ![](pics/jenkins-ansible/provision-update.png)
- edit the .inv file on git-hub to add hosts
    ![](pics/jenkins-ansible/db_inv.png)

# Note - the IP of webserver and Database Insatnces ahev to be updated in dev.inv file & provision.sh file in jenkins ec2 located at /etc/ansible

### pipeline job script:
![](pics/jenkins-ansible/pipeline_script.png)

- the only thing that is different with the last 3 stages is that we need to chage the name of the playbook to run
    - we generate the script using declerative pipeline syntax
    ![](pics/jenkins-ansible/pipeline-script-diff-playbook.png)

### Git-hub Repo:
![](pics/jenkins-ansible/repo.png)
#### Playbooks (3 playbooks)
- Playbook to Install Nginx and set-up Reverse Proxy
    ![](pics/jenkins-ansible/nginx_playbook.png)
- Playbook to Set up Internet facing app
    ![](pics/jenkins-ansible/setup_app_playbook.png)
- Playbook to Set up Database
    ![](pics/jenkins-ansible/setup_db_playbook.png)
#### Inventory file(dev.inv)
![](pics/jenkins-ansible/inventory.png)

    
# Result: manually terminated both app&db instance on aws 
- manually run terraform job to spin up Instances
- manully edited dev.inv on github with IP's of newly spin up instances
- manually edited /etc/ansible?provision.sh on jenkins ec2 with IP of database 
- manually run the ansible job on jenkins



