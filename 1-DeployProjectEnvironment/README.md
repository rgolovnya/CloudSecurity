
### Deployment of the Infrastructure

#### Deploy the S3 buckets:
```
aws cloudformation create-stack --region us-east-1 --stack-name c3-s3 --template-body file://deploy/c3-s3.yml
```

#### Deploy the VPC and Subnets:
```
aws cloudformation create-stack --region us-east-1 --stack-name c3-vpc --template-body file://deploy/c3-vpc.yml
```

#### Deploy the Application stack:
```
aws cloudformation create-stack --region us-east-1 --stack-name c3-app --template-body file://deploy/c3-app.yml --parameters ParameterKey=KeyPair,ParameterValue=<add your key pair name here> --capabilities CAPABILITY_IAM
```

#### Upload data to S3 buckets
```
aws s3 cp ./deploy/free_recipe.txt s3://<BucketNameRecipesFree>/ --region us-east-1
```

```
aws s3 cp ./deploy/secret_recipe.txt s3://<BucketNameRecipesSecret>/ --region us-east-1
```

To access the application at: `http://<ApplicationURL>/free_recipe`

http://c1-web-service-alb-2120860333.us-east-1.elb.amazonaws.com/free_recipe


#### Delete the stack
```
aws cloudformation delete-stack --region us-east-1 --stack-name c3-app
```

#### Extra

```
aws cloudformation create-stack --region us-east-1 --stack-name c3-s3-solution --template-body file://deploy/c3-s3-solution.yml
```

```
aws cloudformation create-stack --region us-east-1 --stack-name c3-app-solution --template-body file://deploy/c3-app_solution.yml --parameters ParameterKey=KeyPair,ParameterValue=ukeypair --capabilities CAPABILITY_IAM
```


#### view the files in the secret recipes bucket
`aws s3 ls  s3://cand-c3-secret-recipes-811631106600/ --region us-east-1`

#### download the files
`aws s3 cp s3://cand-c3-secret-recipes-811631106600/secret_recipe.txt  .  --region us-east-1`


#### Uploading data to S3 buckets

`aws s3 cp free_recipe.txt s3://cand-c3-free-recipes-811631106600/ --region us-east-1`
`aws s3 cp secret_recipe.txt s3://<BucketNameRecipesSecret>/ --region us-east-1`



Result:
```bash
 Exercise_1   project/03-design-for-security ± 
λ aws s3 cp free_recipe.txt s3://<BucketNameRecipesFree>/ --region us-east-1
upload: .\free_recipe.txt to s3://cand-c3-free-recipes-305706552515/free_recipe.txt

 Exercise_1   project/03-design-for-security ± 
λ aws s3 cp secret_recipe.txt s3://cand-c3-secret-recipes-305706552515/ --region us-east-1
upload: .\secret_recipe.txt to s3://cand-c3-secret-recipes-305706552515/secret_recipe.txt
```

#### SSH

`ssh-keygen -t rsa -b 2048`

`chmod 400 ukeypair*`
