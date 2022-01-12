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

## Step 1: set-up Jenkins on aws ec2
-       https://github.com/Viggy005/Jenkins_complete_automation
- follow steps from above link to set-up jenkins server
- follow along to create JOB1: web-hook, merge dev1 into main branch 