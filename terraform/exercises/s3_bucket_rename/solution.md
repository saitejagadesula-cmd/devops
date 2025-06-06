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

```sh
# A bucket name is immutable in AWS so we'll have to create a new bucket
aws s3 mb s3://some-new-bucket-123

# Sync old bucket to new bucket
aws s3 sync s3://some-old-bucket s3://some-new-bucket-123

# Option 1 (remove and import)

## Remove the old bucket from Terraform's state
terraform state rm aws_s3_bucket.some_bucket

## Import new bucket to Terraform's state
terraform import aws_s3_bucket.some_bucket some-new-bucket-123

: '
aws_s3_bucket.some_bucket: Refreshing state... [id=some-new-bucket-123]

Import successful!
The resources that were imported are shown above. These resources are now in
your Terraform state and will henceforth be managed by Terraform.
'

# Option 2 (move)

## Move the old bucket from Terraform's state to the new one
terraform state mv aws_s3_bucket.some_bucket some-new-bucket-123

: '
Move "aws_s3_bucket.some_bucket" to "aws_s3_bucket.some-new-bucket-123"
Successfully moved 1 object(s).
'

# Modify Terraform file

# Modify the Terraform definition to include the new name
# resource "aws_s3_bucket" "some_bucket" {
#    bucket = "some-new-bucket-123"
# }

# Remove old bucket
aws s3 rm s3://some-old-bucket --recursive
aws s3 rb s3://some-old-bucket
```
