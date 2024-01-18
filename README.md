# What we will do?

### We will deploy a static website to aws s3 bucket using terraform in simple 5-STEPS

### All things will be done using TERRAFORM
![terraform-s3](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/97330043-95d2-4720-a451-7f5996566e16)

## For example we will host a simple resume on aws s3 !

### STEP-1 Provider File:
+ Create a provider.tf file and add your provider i.e AWS
+ Also, add the required access keys so that you can access you aws console
> provider.tf
```
provider "aws" {
  region = "ap-south-1"
  access_key = var.access_key
  secret_key = var.secret_key
}
```

### STEP-2 Variable and tfvars file:
+ We are not using the hardcode variables in provider file
+ So, we need to declare variable name and there types in variable file ( optional: default value )
+ To store the variable value we will be using tfvars file
> variable.tf
```
variable "access_key" {
  type = string
}
variable "secret_key" {
  type = string
}
variable "bucketname" {
  type = string
}
```
> terraform.tfvars
```
access_key    = "yourkey"
secret_key    = "yourkey"
bucketname    = "testingbucket0511"
```

### STEP-3 Main File:
+ Create a main.tf file which will be having all the resources and there configurations
> create s3 bucket
```
resource "aws_s3_bucket" "mybucket" {
  bucket = var.bucketname
}
```
> change its ownership and make it public accessible
```
resource "aws_s3_bucket_ownership_controls" "example" {
  bucket = aws_s3_bucket.mybucket.id

  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}
resource "aws_s3_bucket_public_access_block" "example" {
  bucket = aws_s3_bucket.mybucket.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}
```
> enable access control to public-read for s3
```
resource "aws_s3_bucket_acl" "example" {
  depends_on = [
    aws_s3_bucket_ownership_controls.example,
    aws_s3_bucket_public_access_block.example,
  ]

  bucket = aws_s3_bucket.mybucket.id
  acl    = "public-read"
}
```
> create s3 objects index.html, error.html and profile.png which is required for website hosting
> 
> [Click to see index.html and error.html](/)
```
resource "aws_s3_object" "index" {
  bucket = aws_s3_bucket.mybucket.id
  key = "index.html"
  source = "index.html"
  acl = "public-read"
  content_type = "text/html"
}

resource "aws_s3_object" "error" {
  bucket = aws_s3_bucket.mybucket.id
  key = "error.html"
  source = "error.html"
  acl = "public-read"
  content_type = "text/html"
}

resource "aws_s3_object" "profile" {
  bucket = aws_s3_bucket.mybucket.id
  key = "profile.png"
  source = "profile.png"
  acl = "public-read"
}
```
> add s3 website resource and its configuration
```
resource "aws_s3_bucket_website_configuration" "website" {
  bucket = aws_s3_bucket.mybucket.id
  index_document {
    suffix = "index.html"
  }

  error_document {
    key = "error.html"
  }

  depends_on = [ aws_s3_bucket_acl.example ]
}
```
### STEP-4 Run Terraform commands:
+ terraform init - to initialize terraform backend

![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/b1bf4a50-e9e0-4297-a18a-9eac003775a6)


+ terraform plan - which creates an execution plan, that lets you preview the changes that Terraform plans to make to your infrastructure

![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/940ae015-069d-4c76-a89f-ee4608959c46)
![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/85fdc848-fcb4-4a66-a446-edb0948ec8f1)

+ terraform apply - which executes the actions proposed in a terraform plan

![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/1eaa9dba-b14f-4a89-8874-f9069a7e73f1)
![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/409c3c16-539e-44fa-b290-3c303d6c33f5)

### STEP-5  All Done! now check the output on aws console:

+ check the bucket with its objects

![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/d07f9679-bbbc-4b9c-bff8-8c4fb0529a34)

+ check for the website link in properties section

![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/b0d1dcb0-2554-4f70-9832-c79c22b31358)

+ click the link and check your resume

![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/8adc57e3-9bc9-41a1-b528-07b284a2dc3a)

### STEP-6 terraform destroy (OPTIONAL) :
+ you can use terraform destroy as it is a convenient way to destroy all remote objects managed by a particular Terraform configuration.

![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/6ba253c9-2693-44d2-be9c-1d201b76a9bc)
![image](https://github.com/Sumyak-Jain/Basic-Terraform-Project/assets/46700921/3f729d32-3213-469f-964f-945f6ede9521)

#
### CONGRATULATIONS! :clap: now you know how to host a static website on aws s3 using terraform


















