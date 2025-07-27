# AWS S3 CLI Lab - Practice Tasks

## Section 1: Setup and Configuration

**Task**: Set up AWS CLI auto-prompt and check your environment configuration.
**Expected outcome**: Understand AWS CLI interface and verify environment variables are set.

### Answers for Section 1:
```bash
aws s3
aws s3 help
export AWS_CLI_AUTO_PROMPT=on-partial
env | grep AWS
```

---

## Section 2: Bucket Management (Create, Read, Delete)

**Tasks**:
- Create a new S3 bucket in the us-east-1 region
- List all buckets in your AWS account
- Verify that your newly created bucket exists
- Remove an empty bucket from your account
- Remove a bucket and all its contents in one command

### Answers for Section 2:
```bash
# Create a bucket
aws s3 mb s3://my-practice-bucket --region us-east-1
# Alternative (s3api): aws s3api create-bucket --bucket my-practice-bucket --region us-east-1

# List all buckets
aws s3 ls
# Alternative (s3api): aws s3api list-buckets

# Check if bucket exists
aws s3 ls s3://my-practice-bucket
# Alternative (s3api): aws s3api head-bucket --bucket my-practice-bucket

# Delete empty bucket
aws s3 rb s3://my-practice-bucket
# Alternative (s3api): aws s3api delete-bucket --bucket my-practice-bucket

# Force delete bucket with contents
aws s3 rb s3://my-practice-bucket --force
# Alternative (s3api): First delete all objects, then delete bucket
```

---

## Section 3: Object Operations (Upload, Download, Move, Delete)

**Tasks**:
- Upload a local file to your S3 bucket
- Upload all files in a local directory to S3
- Download a specific file from S3 to your local machine
- Download all files from an S3 prefix to a local directory
- Synchronize a local directory with an S3 bucket (upload only changed files)
- Synchronize an S3 bucket with a local directory (download only changed files)
- List all objects in a specific bucket or under a specific path
- Move (rename) an object within the same S3 bucket
- Remove a specific file from your S3 bucket
- Remove all objects from a bucket recursively

### Answers for Section 3:
```bash
# Upload a single file
aws s3 cp local-file.txt s3://my-practice-bucket/
# Alternative (s3api): aws s3api put-object --bucket my-practice-bucket --key local-file.txt --body local-file.txt

# Upload a directory recursively
aws s3 cp local-dir/ s3://my-practice-bucket/ --recursive
# Note: s3api doesn't have a direct equivalent for recursive uploads

# Download a single file
aws s3 cp s3://my-practice-bucket/file.txt local-file.txt
# Alternative (s3api): aws s3api get-object --bucket my-practice-bucket --key file.txt local-file.txt

# Download a directory recursively
aws s3 cp s3://my-practice-bucket/ local-dir/ --recursive
# Note: s3api requires individual get-object calls for each file

# Sync local directory to S3
aws s3 sync local-dir/ s3://my-practice-bucket/
# Note: s3api doesn't have sync functionality

# Sync S3 to local directory
aws s3 sync s3://my-practice-bucket/ local-dir/
# Note: s3api doesn't have sync functionality

# List objects in bucket
aws s3 ls s3://my-practice-bucket/path/
# Alternative (s3api): aws s3api list-objects-v2 --bucket my-practice-bucket --prefix path/

# Move object within bucket
aws s3 mv s3://my-practice-bucket/file.txt s3://my-practice-bucket/moved.txt
# Alternative (s3api): aws s3api copy-object --copy-source my-practice-bucket/file.txt --bucket my-practice-bucket --key moved.txt (then delete original)

# Delete a single object
aws s3 rm s3://my-practice-bucket/file.txt
# Alternative (s3api): aws s3api delete-object --bucket my-practice-bucket --key file.txt

# Delete all objects in bucket
aws s3 rm s3://my-practice-bucket/ --recursive
# Note: s3api requires individual delete-object calls for each file
```

---

## Section 4: Advanced Object Operations

**Tasks**:
- Upload a file with a specific content type specification
- Copy an object from one bucket to another

### Answers for Section 4:
```bash
# Upload with content type (s3api preferred for more control)
aws s3api put-object --bucket my-practice-bucket --key images/hello.txt --content-type text/plain --body hello.txt
# Alternative (s3): aws s3 cp hello.txt s3://my-practice-bucket/images/ --content-type text/plain

# Copy object between buckets
aws s3 cp s3://source-bucket/file.txt s3://dest-bucket/file.txt
# Alternative (s3api): aws s3api copy-object --copy-source source-bucket/path/to/file.txt --bucket dest-bucket --key path/to/file.txt
```

---

## Section 5: Access and Security Operations

**Tasks**:
- Create a temporary URL for accessing a private S3 object

### Answers for Section 5:
```bash
# Generate presigned URL
aws s3 presign s3://my-practice-bucket/file.txt --expires-in 3600
# Note: s3api doesn't have a direct presign command
```

---

## Section 6: Query and Filter Operations (s3api)

**Tasks**:
- Retrieve complete bucket information in JSON format
- Extract just the bucket names from the list-buckets output
- Display bucket names in plain text format (one per line)
- Display bucket names and creation dates in a formatted table
- Show only buckets whose names contain 'example'
- Extract only the creation dates of all buckets
- Show names of buckets created after January 1, 2023
- Get the total number of buckets in your account
- Get the name of the bucket that was created first
- Get the name of the most recently created bucket
- Query for a specific bucket by name using JMESPath
- Get all object keys in a bucket using list-objects-v2

### Answers for Section 6:
```bash
# Get all bucket details
aws s3api list-buckets --query Buckets

# Get only bucket names
aws s3api list-buckets --query Buckets[].Name

# Get bucket names as text
aws s3api list-buckets --query Buckets[].Name --output text

# Custom table output
aws s3api list-buckets --query 'Buckets[].{Name: Name, MyDate: CreationDate}' --output table

# Filter buckets by name pattern
aws s3api list-buckets --query 'Buckets[?contains(Name, `example`)].Name' --output yaml

# Get creation dates only
aws s3api list-buckets --query 'Buckets[].CreationDate' --output text

# Filter by date range
aws s3api list-buckets --query 'Buckets[?CreationDate>`2023-01-01`].Name' --output json

# Count total buckets
aws s3api list-buckets --query 'Buckets | length(@)' --output text

# Find oldest bucket
aws s3api list-buckets --query 'Buckets | sort_by(@, &CreationDate) | [0].Name' --output text

# Find newest bucket
aws s3api list-buckets --query 'Buckets | sort_by(@, &CreationDate) | [-1].Name' --output yaml

# Filter specific bucket
aws s3api list-buckets --query "Buckets[?Name=='my-specific-bucket'].Name"

# List object keys
aws s3api list-objects-v2 --bucket my-practice-bucket --query Contents[].Key
```

---

## Section 7: Comprehensive Practice Scenarios

**Scenarios**:
- Complete Bucket Lifecycle: create → upload files → list contents → download → clean up
- Backup and Restore: sync a local directory to S3, then restore it to a different location
- Cross-Bucket Operations: practice moving data between different buckets

### Answers for Section 7:
**Scenario 7.1 Order of operations**:
1. Create bucket: `aws s3 mb s3://lifecycle-bucket`
2. Upload single file: `aws s3 cp file.txt s3://lifecycle-bucket/`
3. Upload directory: `aws s3 cp local-dir/ s3://lifecycle-bucket/ --recursive`
4. List bucket contents: `aws s3 ls s3://lifecycle-bucket/`
5. Download a file: `aws s3 cp s3://lifecycle-bucket/file.txt downloaded-file.txt`
6. Delete objects: `aws s3 rm s3://lifecycle-bucket/ --recursive`
7. Delete bucket: `aws s3 rb s3://lifecycle-bucket`

**Scenario 7.2 Order of operations**:
1. Create source directory with files
2. Sync to S3: `aws s3 sync source-dir/ s3://backup-bucket/`
3. Create new empty directory
4. Sync from S3 to new directory: `aws s3 sync s3://backup-bucket/ restore-dir/`
5. Verify contents match

**Scenario 7.3 Order of operations**:
1. Create two buckets: `aws s3 mb s3://bucket1` and `aws s3 mb s3://bucket2`
2. Upload files to first bucket: `aws s3 cp files/ s3://bucket1/ --recursive`
3. Copy objects to second bucket: `aws s3 cp s3://bucket1/ s3://bucket2/ --recursive`
4. Verify contents in both buckets: `aws s3 ls s3://bucket1/` and `aws s3 ls s3://bucket2/`
5. Clean up both buckets: `aws s3 rb s3://bucket1 --force` and `aws s3 rb s3://bucket2 --force`