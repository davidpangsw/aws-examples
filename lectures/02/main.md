# Main

## AWS S3 CLI common operations
### S3 Commands
```bash
# This script demonstrates basic AWS S3 CLI operations.

# Basic s3 commands, cli auto prompt
aws s3
aws s3 help
aws s3 ls 
export AWS_CLI_AUTO_PROMPT=on-partial  # Enable auto prompt
env | grep AWS


# Bucket Operations
aws s3 mb s3://my-bucket-name --region us-east-1  # Make a bucket
aws s3 ls                                         # List all buckets
aws s3 ls s3://my-bucket-name                     # Check if bucket exists
aws s3 rb s3://my-bucket-name                     # Remove an empty bucket
aws s3 rb s3://my-bucket-name --force             # Remove bucket and contents

# Object Operations
aws s3 cp local-file.txt s3://my-bucket-name/     # Upload a file
aws s3 cp local-dir/ s3://my-bucket-name/ --recursive # Upload all the files in a directory
aws s3 cp s3://my-bucket-name/file.txt local-file.txt # Download a file
aws s3 cp s3://my-bucket-name/ local-dir/ --recursive # Download a directory
aws s3 sync local-dir/ s3://my-bucket-name/       # Sync local directory to bucket
aws s3 sync s3://my-bucket-name/ local-dir/       # Sync bucket to local directory
aws s3 ls s3://my-bucket-name/path/               # List objects in bucket under path
aws s3 mv s3://my-bucket-name/file.txt s3://my-bucket-name/moved.txt # Move object within bucket
aws s3 rm s3://my-bucket-name/file.txt            # Remove a file

# Access Operations
aws s3 presign s3://my-bucket-name/file.txt --expires-in 3600 # Generate presigned URL for private file
```


### S3API
```bash
#!/bin/bash

# This script demonstrates basic AWS S3 CLI operations using s3api.

# Bucket Operations
aws s3api create-bucket --bucket my-bucket-name --region us-east-1
aws s3api list-buckets # List all buckets
aws s3api head-bucket --bucket my-bucket-name # Check if bucket exists
aws s3api delete-bucket --bucket my-bucket-name # Remove an empty bucket

# Object Operations
aws s3api put-object --bucket my-bucket-name --key path/to/file.txt --body local-file.txt # Upload a file
aws s3api get-object --bucket my-bucket-name --key path/to/file.txt local-file.txt # Download a file
aws s3api list-objects-v2 --bucket my-bucket-name --prefix path/ # List objects in bucket under path
aws s3api copy-object --copy-source my-source-bucket/path/to/file.txt --bucket my-dest-bucket --key path/to/file.txt
aws s3api delete-object --bucket my-bucket-name --key path/to/file.txt # Remove a file

# Query bucket information using JMES Path
aws s3api list-buckets --query Buckets # Get all bucket details in JSON
aws s3api list-buckets --query Buckets[].Name # Get all bucket names
aws s3api list-buckets --query Buckets[].Name --output text # Bucket names in text
aws s3api list-buckets --query 'Buckets[].{Name: Name, MyDate: CreationDate}' --output table # Bucket names and creation dates in table
aws s3api list-buckets --query 'Buckets[?contains(Name, `example`)].Name' --output yaml # Bucket names containing 'example' in YAML
aws s3api list-buckets --query 'Buckets[].CreationDate' --output text # Creation dates in text
aws s3api list-buckets --query 'Buckets[?CreationDate>`2023-01-01`].Name' --output json # Names of buckets created after 2023-01-01
aws s3api list-buckets --query 'Buckets | length(@)' --output text # Count total buckets
aws s3api list-buckets --query 'Buckets | sort_by(@, &CreationDate) | [0].Name' --output text # Name of the oldest bucket
aws s3api list-buckets --query 'Buckets | sort_by(@, &CreationDate) | [-1].Name' --output yaml # Name of the newest bucket
```
