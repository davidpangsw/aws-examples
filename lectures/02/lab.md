# AWS S3 CLI Lab - Practice Tasks

## Section 1: Setup and Configuration

**Tasks**:
- Display AWS S3 command help and options
- Set up AWS CLI auto-prompt for command assistance
- Check your AWS environment variables

**Questions**:
- What does `AWS_CLI_AUTO_PROMPT=on-partial` do?
- How can you get help for specific S3 commands?

### Answers for Section 1:
```bash
# Basic S3 commands and help
aws s3
aws s3 help
export AWS_CLI_AUTO_PROMPT=on-partial
env | grep AWS
```

**Answer to Questions**:
- `AWS_CLI_AUTO_PROMPT=on-partial` enables interactive command completion and suggestions
- Use `aws s3 help` for general help, or `aws s3 <command> help` for specific command help

---

## Section 2: Bucket Operations (Create, Read, Delete)

**Tasks**:
- Create a new S3 bucket with region specification
- List all buckets in your account
- Check if a specific bucket exists
- Remove an empty bucket
- Force remove a bucket with all its contents

**Questions**:
- Why should you specify a region when creating buckets?
- What's the difference between `rb` and `rb --force`?
- When would you use s3api instead of s3 for bucket operations?

### Answers for Section 2:
```bash
# Create bucket
aws s3 mb s3://my-bucket-name --region us-east-1
# Alternative (s3api): aws s3api create-bucket --bucket my-bucket-name --region us-east-1

# List all buckets
aws s3 ls
# Alternative (s3api): aws s3api list-buckets

# Check if bucket exists
aws s3 ls s3://my-bucket-name
# Alternative (s3api): aws s3api head-bucket --bucket my-bucket-name

# Remove empty bucket
aws s3 rb s3://my-bucket-name
# Alternative (s3api): aws s3api delete-bucket --bucket my-bucket-name

# Remove bucket and all contents
aws s3 rb s3://my-bucket-name --force
# Alternative (s3api): requires deleting all objects first, then bucket
```

**Answer to Questions**:
- Region specification ensures bucket is created in desired geographic location for compliance/latency
- `rb` only removes empty buckets; `rb --force` removes bucket and all objects
- Use s3api when you need more control over bucket configuration or metadata

---

## Section 3: Object Operations (Upload, Download, Move, Delete)

**Tasks**:
- Upload a single file to S3
- Upload an entire directory recursively
- Download a single file from S3
- Download an entire directory recursively
- Move/rename an object within the same bucket
- Delete a single object from S3

**Questions**:
- What's the difference between `cp` and `sync` commands?
- How does recursive copying work?
- When would you use `mv` vs `cp` for S3 objects?

### Answers for Section 3:
```bash
# Upload single file
aws s3 cp local-file.txt s3://my-bucket-name/
# Alternative (s3api): aws s3api put-object --bucket my-bucket-name --key local-file.txt --body local-file.txt

# Upload directory recursively
aws s3 cp local-dir/ s3://my-bucket-name/ --recursive
# Alternative (s3api): no direct equivalent - requires multiple put-object calls

# Download single file
aws s3 cp s3://my-bucket-name/file.txt local-file.txt
# Alternative (s3api): aws s3api get-object --bucket my-bucket-name --key file.txt local-file.txt

# Download directory recursively
aws s3 cp s3://my-bucket-name/ local-dir/ --recursive
# Alternative (s3api): no direct equivalent - requires multiple get-object calls

# Move object within bucket
aws s3 mv s3://my-bucket-name/file.txt s3://my-bucket-name/moved.txt
# Alternative (s3api): aws s3api copy-object then delete-object

# Delete single object
aws s3 rm s3://my-bucket-name/file.txt
# Alternative (s3api): aws s3api delete-object --bucket my-bucket-name --key file.txt
```

**Answer to Questions**:
- `cp` copies files; `sync` only transfers files that are different or missing
- `--recursive` flag processes all files in subdirectories
- `mv` moves (deletes source); `cp` copies (keeps source)

---

## Section 4: Synchronization Operations

**Tasks**:
- Sync a local directory to S3 bucket
- Sync S3 bucket to local directory
- Compare sync behavior with recursive copy

**Questions**:
- How does S3 sync determine which files to transfer?
- What are the advantages of sync over recursive copy?
- Why doesn't s3api have sync functionality?

### Answers for Section 4:
```bash
# Sync local directory to S3
aws s3 sync local-dir/ s3://my-bucket-name/
# Alternative (s3api): no equivalent - sync is high-level operation

# Sync S3 to local directory
aws s3 sync s3://my-bucket-name/ local-dir/
# Alternative (s3api): no equivalent - would need to compare and transfer manually
```

**Answer to Questions**:
- Sync compares file sizes and modification times to transfer only changed files
- Sync is more efficient for large directories as it skips unchanged files
- s3api is low-level and doesn't include batch operations like sync

---

## Section 5: Object Listing and Path Operations

**Tasks**:
- List objects in a bucket under a specific path/prefix
- Practice listing with different path structures

**Questions**:
- How do S3 "folders" work compared to filesystem directories?
- What role does the prefix play in listing operations?

### Answers for Section 5:
```bash
# List objects under specific path
aws s3 ls s3://my-bucket-name/path/
# Alternative (s3api): aws s3api list-objects-v2 --bucket my-bucket-name --prefix path/
```

**Answer to Questions**:
- S3 has a flat structure; "folders" are just key prefixes ending with "/"
- Prefix filters objects to show only those starting with the specified string

---

## Section 6: Access and Security Operations

**Tasks**:
- Generate a presigned URL for temporary access to a private object
- Set expiration time for the presigned URL

**Questions**:
- What are presigned URLs used for?
- How do expiration times work with presigned URLs?
- Can s3api generate presigned URLs?

### Answers for Section 6:
```bash
# Generate presigned URL
aws s3 presign s3://my-bucket-name/file.txt --expires-in 3600
# Note: s3api doesn't have direct presign functionality
```

**Answer to Questions**:
- Presigned URLs allow temporary access to private S3 objects without AWS credentials
- Expiration time (in seconds) determines how long the URL remains valid
- s3api doesn't have presign; it's a high-level s3 feature

---

## Section 7: Advanced Object Operations (s3api)

**Tasks**:
- Copy objects between different buckets using s3api
- Upload with specific key path using s3api
- Use head-bucket to check bucket existence

**Questions**:
- When would you prefer s3api over s3 commands?
- What additional control does s3api provide?

### Answers for Section 7:
```bash
# Copy between buckets (s3api method)
aws s3api copy-object --copy-source my-source-bucket/path/to/file.txt --bucket my-dest-bucket --key path/to/file.txt
# Alternative (s3): aws s3 cp s3://my-source-bucket/file.txt s3://my-dest-bucket/file.txt

# Upload with specific key path
aws s3api put-object --bucket my-bucket-name --key path/to/file.txt --body local-file.txt
# Alternative (s3): aws s3 cp local-file.txt s3://my-bucket-name/path/to/file.txt

# Check bucket existence
aws s3api head-bucket --bucket my-bucket-name
# Alternative (s3): aws s3 ls s3://my-bucket-name
```

**Answer to Questions**:
- Use s3api when you need precise control over metadata, headers, or API-specific features
- s3api provides access to all S3 REST API features, custom headers, and detailed responses

---

## Section 8: JMESPath Query Operations

**Tasks**:
- Get all bucket details in JSON format
- Extract only bucket names
- Display bucket names in different output formats
- Filter buckets by name pattern
- Get bucket creation dates
- Filter buckets by creation date
- Count total number of buckets
- Find oldest and newest buckets

**Questions**:
- What is JMESPath and why is it useful?
- What output formats are available in AWS CLI?
- How do JMESPath filters work?

### Answers for Section 8:
```bash
# Get all bucket details in JSON
aws s3api list-buckets --query Buckets

# Get only bucket names
aws s3api list-buckets --query Buckets[].Name

# Bucket names in text format
aws s3api list-buckets --query Buckets[].Name --output text

# Bucket names and dates in table format
aws s3api list-buckets --query 'Buckets[].{Name: Name, MyDate: CreationDate}' --output table

# Filter buckets containing 'example'
aws s3api list-buckets --query 'Buckets[?contains(Name, `example`)].Name' --output yaml

# Get only creation dates
aws s3api list-buckets --query 'Buckets[].CreationDate' --output text

# Filter buckets created after specific date
aws s3api list-buckets --query 'Buckets[?CreationDate>`2023-01-01`].Name' --output json

# Count total buckets
aws s3api list-buckets --query 'Buckets | length(@)' --output text

# Find oldest bucket
aws s3api list-buckets --query 'Buckets | sort_by(@, &CreationDate) | [0].Name' --output text

# Find newest bucket
aws s3api list-buckets --query 'Buckets | sort_by(@, &CreationDate) | [-1].Name' --output yaml
```

**Answer to Questions**:
- JMESPath is a query language for JSON that allows filtering and transforming AWS CLI output
- Available formats: json, text, table, yaml, yaml-stream
- JMESPath filters use `[?condition]` syntax to select array elements matching criteria

---

## Section 9: Practical Scenarios

**Scenario Questions**:
1. You need to backup a project directory to S3, then restore it elsewhere. What commands would you use?

2. How would you find all buckets created in the last year?

3. What's the most efficient way to upload 1000 files to S3?

4. How would you temporarily share a private file with someone who doesn't have AWS access?

**Scenario Answers**:

1. **Backup and Restore**:
   ```bash
   # Backup
   aws s3 sync ./project/ s3://backup-bucket/project-backup/
   
   # Restore elsewhere
   aws s3 sync s3://backup-bucket/project-backup/ ./restored-project/
   ```

2. **Find Recent Buckets**:
   ```bash
   aws s3api list-buckets --query 'Buckets[?CreationDate>`2024-01-01`].Name'
   ```

3. **Bulk Upload**: Use `aws s3 sync` or `aws s3 cp --recursive` for efficient parallel uploads

4. **Temporary Sharing**: Use `aws s3 presign` to generate a time-limited URL

---

## Key Concepts Summary

**s3 vs s3api**:
- **s3**: High-level commands, convenient for common operations, includes batch operations
- **s3api**: Low-level commands, direct API access, more control over requests

**Best Practices**:
- Use s3 commands for everyday file operations
- Use s3api when you need specific API features or metadata control
- Always specify regions for bucket creation
- Use sync for efficient large-scale transfers
- Use JMESPath queries to filter and format AWS CLI output

**Command Patterns**:
- `aws s3 <operation>` for high-level operations
- `aws s3api <api-call>` for low-level API access
- `--query` for filtering output with JMESPath
- `--output` for choosing response format