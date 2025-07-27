# Lab generation requirements

## Command Preference

When tasks can be performed by both `s3` and `s3api`:
- **Primary focus**: Use high-level `s3` commands (preferred approach)
- **Secondary alternative**: Provide `s3api` commands as alternatives in comments
- **Format**: Show `s3` command first, then `# Alternative (s3api): command`

## AWS S3 Specifics

- **Bucket operations**: Emphasize the difference between high-level and low-level APIs
- **Query operations**: For s3api queries, include various JMESPath examples
- **Sync operations**: Note when s3api doesn't have equivalent functionality
- **Practical scenarios**: Include real-world workflow examples (backup, restore, cross-bucket operations)
