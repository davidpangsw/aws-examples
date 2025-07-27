# Lecture
```bash
aws s3
aws s3 help
aws s3 ls 
export AWS_CLI_AUTO_PROMPT=on-partial  # Enable auto prompt
env | grep AWS

# remove bucket (remove content before that)
aws s3 rb s3://davidpangsw-s3-example-bucket # fail if non-empty
aws s3 rm s3://davidpangsw-s3-example-bucket/ --recursive
aws s3 rb s3://davidpangsw-s3-example-bucket

# create and list buckets
# If you don't specify region, AWS CLI uses your default region from the AWS configuration (`~/.aws/config` or environment variable `AWS_DEFAULT_REGION`).
aws s3api create-bucket --bucket davidpangsw-s3-example --region us-east-1
aws s3api list-buckets

# Query with JMESPath
aws s3api list-buckets --query Buckets
aws s3api list-buckets --query Buckets[].Name
aws s3api list-buckets --query Buckets[].Name --output text
aws s3api list-buckets --query Buckets[].Name --output table #json/text/table/yaml/yaml-stream
aws s3api list-buckets --query "Buckets[?Name=='davidpangsw-s3-example'].Name"
aws s3api list-buckets --query "Buckets[?Name=='davidpangsw-s3-example'].Name"

# Put / List / Get objects
# ListObjectsV2 uses ContinuationToken for pagination, which is a more robust and efficient method for retrieving large object lists compared to the Marker parameter used in ListObjects. ContinuationToken ensures consistent and reliable pagination, especially in scenarios with concurrent object modifications.
aws s3api put-object --bucket davidpangsw-s3-example --key images/hello.txt --content-type plain/txt --body hello.txt
aws s3api list-objects-v2 --bucket davidpangsw-s3-example --query Contents[].Key
aws s3api get-object --bucket davidpangsw-s3-example --key images/hello.txt hello.txt

## Note: s3api is low-level, and doesn't provide options to upload whole folder?
aws s3 sync images/ s3://davidpangsw-aws-example/images/
```
