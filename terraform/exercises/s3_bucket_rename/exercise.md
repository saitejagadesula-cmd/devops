# Rename S3 Bucket

## Requirements

* An existing S3 bucket tracked by Terraform.
If you don't have it, you can use the following block and run `terraform apply`:

```terraform
resource "aws_s3_bucket" "some_bucket" {
    bucket = "some-old-bucket"
}
```

Attention: Since S3 buckets are globally unique, you will likely have to rename the bucket as someone else might have named it that way already.

## Objectives

1. Rename an existing S3 bucket and make sure it's still tracked by Terraform

## Solution

Click [here to view the solution](solution.md)
