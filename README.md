# What we will do?

### We will deploy a static website to aws s3 bucket using terraform

### All things will be done using TERRAFORM
![terraform-s3](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/97330043-95d2-4720-a451-7f5996566e16)

## For example we will host a simple resume on aws s3 !
[To see all the source code click!](/)
### STEP-1 Provider File:
+ Create a provider.tf file and add your provider i.e AWS
+ Also, add the required access keys so that you can access you aws console
```
> provider.tf
provider "aws" {
  region = "ap-south-1"
  access_key = var.access_key
  secret_key = var.secret_key
}
```
### STEP-2 Variable and tfvar file:
+ We are not using the hardcode variables in provider files
+ So, we need to declare variable name and there types in variable file
+ Also, add the required access keys so that you can access you aws console
```
provider "aws" {
  region = "ap-south-1"
  access_key = var.access_key
  secret_key = var.secret_key
}
```



