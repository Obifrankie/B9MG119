# Cloud Technology for Business (B9MG119) 
This repo contains the Cloud Strategy and Sample Deployment of an organization called BlueMoon

## Background of BlueMoon
BlueMoon is a startup that has been operational for the last five years in the Ghana area of West-Africa. They have experienced a fairly average amount of growth for the last few years but the number of their customer base is wasily quantifiable. BlueMoon is a fintech, they collaborate with Banks to provide a credit model to give out loans to users. This model rates users and gives them a score, with this score the bank can determine if to give a loan to said user or reject the loan. It also helps them decide the range of amount loan a user is eligible for. Asides Collaborating with banks BlueMoon also operates it's own loan management system which it uses to give loans directly to it's customer base. BlueMoon has a current monthly spend of over $5,036 BlueMoon have noticed that they are over spending for their cloud infrastructure and would like to reduce cost while also optimizing their cloud usage

## Current IT setup
BlueMoon used to have an engineering team of about 25 engineers but recently they have downsized and they currently they have a team of 12 engineers. The team includes 
- 2 Backends
- 2 Frontends
- 1 DevOps
- 3 Mobile Devs(ios/andriod)
- 2 Testers
- 1 Customer Success
- 2 Product Managers
- 1 Product Designer
- 1 CTO
- 

The team tries to follow the Agile Methodology; they have daily standup, demo days, sprints, a sprint board, backlogs e.t.c

BlueMoon uses AWS as it's default cloud provider. Below is a list of it's current architecture
- 9 EC2 instances
- Auto-Scaling for both EC2 and RDS
- 15 RDS instances
- over a 100 S3 buckets
- 49 IAM users
- 9 Access Keys
- 1 Kubernetes Cluster
- 2 VPC
- 27 security groups
- Domains Hosted in GoDaddy
- Grafana & Prometheus hosted on an EC2 instance 

## Recommendations 
After careful review and monitoring of the organizations we have been able to deduce that the organization is over spending on it cloud usage. The current Billing of the organisation is $5,036 which is a lot considering the size of the organisation and the quantity of users they serve. Our current recommendation are as follows. The recommendations is going to be grouped under 2 major categories 

### Cost
- Delete EC2 instances that does not serve any purpose
- Remove the auto-scaling
- Delete RDS instances that does not serve any purpose
- Delete the Kubernetes Cluster
- Terminate the EC2 instance hosting grafana and prometheus

### Monitoring 
- Install cloudwatch agent to gather metrics for the instances
- Configure AWS budgest and thresh holds to monitor cost and send an alarm when cost has passed a certain threshold. This would allow you be untop of your cloud spendings
- Configure AWS cost anomalies. This monitors your cloud spending over time and reports if it notices a strange spike in your cloud spending
- Configure Metric Dashborads on cloudwatch

### Performance 
- Move DNS hosted on Godaddy to Route 53 this will aloow you to manage all your resources in one place
- Configure cloud Metrics Dashboard on cloudwatch this gives you greater view to how your performance is performing
- Review the S3 Buckets delete the empty buckets or buckets that don't serve any purpose
- Review the S3 Buckets and move buckets that don't need to be readily available to other tiers
- Review the security groups and delete redundant security groups this would allow for better management and connectivity of internal resources
- Create AWS Backup vaults for the infrastructure to easily recover it


### Security
- Look through your IAM users and review the accounts that are needed and delete the once that are not needed. This is to reduce tha amount of people that has access to your AWS environment
- Review the IAM policy and access level attached to each IAM users
- Review the EC2 keys and delete the redundant keys
- Review the Access Levels on each key
- Use Secret Manager to manage secret values
- Enable 2FA for all accounts

The tools that would be used to achieve the current goals are:
- Cloudwatch: this is an AWS SaaS offering to help you monitor and gain metrics on your resources 
- IAM: We will use to grant granular control to principals 
- AWS Billing and Cost management: This is a SaaS solution it is AWS cost center to aggregate all your cloud spending
- EC2: This is AWS compute service it is a IaaS offering
- RDS: This is AWS relational database offering it is a PaaS service.





