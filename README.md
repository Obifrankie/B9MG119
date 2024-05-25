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

Of the 9 EC2 instances only 3 are actually of importance for everday use and they are:
- 1 EC2 instance for production api (Prod Env)
- 1 EC2 instance for testing api (Test Env)
- 1 EC2 instance hosting the company website
The other EC2 instances where created for some other purposes that were abandoned at some point but this EC2 instances were not decommisioned anf they have been paying bills on this purposeless instances. They have an instance solely dedicated for renewing their SSL certificates, this is not an appropriate use of an EC2 instance

The Auto-Scaling is currently not serving any purpose because they are only serving only a small customer base in Ghana. This should be removed 

Of the 15 RDS instances, there are 3 clusters. The clusters are mainly save this prposes:
- The Production RDS cluster is the cluster for production workloads
- The Test RDS cluster is the cluster for Test workloads
- The Web cluster is the cluster for the Website
Some RDS instances are only serving a single purpose hosting a single database and by so doing under utilizing the RDS instances. This databases should be merged into a single database and decommision the redundant databases

The 2 VPC are serving the the Production and Test Environments respectively.

The IAM Service is the companies default Identity service for their cloud environment



## Recommendations 
After careful review and monitoring of the organizations we have been able to deduce that the organization is over spending on it cloud usage. The current Billing of the organisation is $5,036 which is a lot considering the size of the organisation and the quantity of users they serve. Our current recommendation are as follows. The recommendations is going to be grouped under 2 major categories 

### Cost
- Delete EC2 instances that does not serve any purpose: we are doing this because after looking at the environment we noticed that some instances were not serving any particular purpose.
  
- Merge EC2 instances: Some instances had nothing in particular running on them, some instances where used for the sole purpose of just renewing SSL certificates. This is not a proper use of an instance the SSL certificates can be moved to the instances hosting the Test and Production instances. Test certificates should be moved to the Test instance and the Production certificates should be moved to the Production instances by so doing we are cutting down the costs we incure paying for instances that are not serving any purposes.

- Migrate databases: We noticed that some RDS instances were running single databases and by so doing greatly under-utilising the database instances. We are going to migrate this single databases to the Test database and the Prod database. All test related databases would be migrated into the Test cluster all Prod related databases would be migrated to the Prod cluster. This way we would be cutting down costs we incure for paying for under-utilizing databases
  
- Delete RDS instances that does not serve any purpose: We are doig this because we noticed that some RDS instances are not serving any purpose by so doing we are not paying for RDS instances we are actually not using.

- Delete the Kubernetes Cluster: The Kubernetes cluster is currently not being used it was being provisioned for a use case but was abandoned half-way but the instance was not decommisioned and BlueMoon has been paying for this cluster while not using it.

- Terminate the EC2 instance hosting grafana and prometheus: We can use cloudwatch to currently fulfill BlueMoon's monitoring use case. The Grafana and Prometheus tools are currently being hosted on an EC2 instance although this is recommended and would good addittion to BlueMoon monitoring systems, we would have to decomission them for now because it is currently on being used to monitor one EC2 instance and we are incuring cost running this EC2 instance used to host the grafana and Prometheus tools

### Monitoring 
- Install cloudwatch agent to gather metrics for the instances: We are doing this so we can have a robust monitoring for the instances. we are doing this so we can gather logs and metrics and have better insights on our instances. We are going to use the logs and metrics gathered by the cloudwatch agent and create dashboards on AWS cloudwatch. We can create a host of dashboards using this metrics ranging from health, CPU utilisation, Storage, heartbeat and most importantly we can gather custom metrics and logs on BlueMoon services that are hosted on this EC2 instances. With this metrics we are gathering we can configure alarms to alert us if any of BlueMoon service is doing poorly and also if any problems might occur with our instances so we can mitigate it before it before it causes a Prod outage 

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
- - Remove the auto-scaling: We are doing this because BlueMoon currently do not require auto-scaling because they are currently serving a small customer base and we are not expecting an unexpected increase in requests in the nearest future. To also mitigate a case were we might need to scale up



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
- Instance Scheduler 





