### Udacity Cloud Architect Nanodegree
### Project 3: Cloud Security - Secure the Recipe Vault Web Application

In this project, you will:

-	Deploy and assess a simple web application environment’s security posture
-	Test the security of the environment by simulating attack scenarios and exploiting cloud configuration vulnerabilities
-	Implement monitoring to identify insecure configurations and malicious activity
-	Apply methods learned in the course to harden and secure the environment
-	Design a DevSecOps pipeline

## Exercise 1 - Deploy Project Environment
 
**_Deliverables for Exercise 1:_**
* **E1T4.txt** - Text file identifying 2 poor security practices with justification. 
 
### Task 1:  Review Architecture Diagram
In this task, the objective is to familiarize yourself with the starting architecture diagram. An architecture diagram has been provided which reflects the resources that will be deployed in your AWS account.
 
The diagram file, title `AWS-WebServiceDiagram-v1-insecure.png`, can be found in the _starter_ directory in this repo.
 
![base environment](./starter/AWS-WebServiceDiagram-v1-insecure.png)
 
#### Expected user flow:
- Clients will invoke a public-facing web service to pull free recipes.  
- The web service is hosted by an HTTP load balancer listening on port 80.
- The web service is forwarding requests to the web application instance which listens on port 5000.
- The web application instance will, in turn, use the public-facing AWS API to pull recipe files from the S3 bucket hosting free recipes. An IAM role and policy will provide the web app instance permissions required to access objects in the S3 bucket.
- Another S3 bucket is used as a vault to store secret recipes; there are privileged users who would need access to this bucket. The web application server does not need access to this bucket.
 
#### Attack flow:
- Scripts simulating an attack will be run from a separate instance which is in an un-trusted subnet.
- The scripts will attempt to break into the web application instance using the public IP and attempt to access data in the secret recipe S3 bucket.
 
### Task 2: Review CloudFormation Template
In this task, the objective is to familiarize yourself with the starter code and to get you up and running quickly. Spend a few minutes going through the .yml files in the _starter_ folder to get a feel for how parts of the code will map to the components in the architecture diagram. 
 
Additionally, we have provided a CloudFormation template which will deploy the following resources in AWS:
 
#### VPC Stack for the underlying network:
* A VPC with 2 public subnets, one private subnet, and internet gateways etc for internet access.
 
#### S3 bucket stack:
* 2 S3 buckets that will contain data objects for the application.
 
#### Application stack:
* An EC2 instance that will act as an external attacker from which we will test the ability of our environment to handle threats
* An EC2 instance that will be running a simple web service.
* Application LoadBalancer
* Security groups
* IAM role
 

### Task 3: Deployment of the Infrastructure

#### Deploy the S3 buckets:
```
aws cloudformation create-stack --region us-east-1 --stack-name c3-s3 --template-body file://provision/c3-s3.yml
```

#### Deploy the VPC and Subnets:
```
aws cloudformation create-stack --region us-east-1 --stack-name c3-vpc --template-body file://provision/c3-vpc.yml
```

#### Deploy the Application stack:
```
aws cloudformation create-stack --region us-east-1 --stack-name c3-app --template-body file://provision/c3-app.yml --parameters ParameterKey=KeyPair,ParameterValue=<add your key pair name here> --capabilities CAPABILITY_IAM
```

#### Upload data to S3 buckets
```
aws s3 cp ./content/free_recipe.txt s3://<BucketNameRecipesFree>/ --region us-east-1
```

```
aws s3 cp ./content/secret_recipe.txt s3://<BucketNameRecipesSecret>/ --region us-east-1

