AWS Lambda function Terraform module
====================================

Terraform module wich creates a Lambda Function with VPC support in AWS

> Note: All credits go here for https://github.com/adampolomski from I took this module.
> I have just modufied it for my need:wq!s

Usage
-----

```hcl
module "function" {
  source = "git::https://github.com/egarbi/tf_aws_lambda_aliased"
  function_name = "example"
  environment   = "testing"
  handler = "com.lebara.example.handler.MainHandler::handle"
  s3_bucket = "example-lambda-bucket"
  s3_builds_prefix = "builds/"
  build_path = "../target/example-1.jar"
  description = "Just showing a fictional example here"
  timeout = "300"
  memory_size = "512"
  vpc_config = {
    security_group_ids = [
      "vpc-XXXXXXXX"]
    subnet_ids = [
      "subnet-XXXXXXX"]
  }
  function_variables = {
    YOUR_VARABLES_HERE = "somevalue"
  }
  // Policy below will allow the Lambda function to execute while accessing a resource within a VPC
  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Action": [
        "ec2:CreateNetworkInterface",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DeleteNetworkInterface"
        ],
        "Resource": "*"
    },
  ]
}
EOF
}
```
