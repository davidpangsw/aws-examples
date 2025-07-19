# Introduction
https://www.youtube.com/watch?v=c3Cn4xYfxJYß

https://www.exampro.co/saa-c03

## Basic Certs
- Can study at the same time, many overlap
    - Solution Architect Associate
    - SysOps Administrator Associate (Cloud Networking, Infrastructure Automation, Servers)
    - Developer Associate (Developer Tools, Deployment, Applications, Programming, APIs)

- 60 hours (if you have AWS Cloud Practitioner)
- Average 24 hours
    - 1 hour every day
- 睇完course就去呢到做題目
    - https://www.examtopics.com/exams/amazon/aws-certified-solutions-architect-associate-saa-c03/
    - Examtopic.com is considered exam dump site that can get your certification revoked.  Use it if you want but don't tell people or advertise it.  Most exam discussion discord or reddit banned the mentioning of examtopic.com or so similar exam dump sites
    - Paid online practice exams


##
- 30% Design Secure Architecture
    - Actually not very focused
- 26% Design Resilient Architecture
- 24% Design High-Performing Architecture
- 20% Cost Optimized Architectures


##
In person test centre / online
Person Vue online

##
720 / 1000 to pass
around 72% (scaled scoring)
65 Questions, 15 are unscored
    - Some questions are very hard to catch cheaters
MC, Multiple Answers
2.1 hour (130 mins)? ~2 min per question? (varies, check it)

##
Cert valid for 36 months (3 years)

##
It is not sufficient for a job. Go:
- Learn other 2 basic certs
- Write some personal projects

##
- Some services are broken. But the troubleshooting process is important.
- Clean up costly infrastruture!!! Be proactive and check if resources are left running. No $$ at all.



# Gitpod
- Setting up Github PAT to restrict Gitpod in one repo (30days max)
    - Remember to get a PAT token in github -> Developer Settings -> Fine-grained tokens (PAT)
```sh
gp env GITHUB_TOKEN="your pat token"
git config --global credential.helper store
git remote set-url origin https://davidpangsw:$GITHUB_TOKEN@github.com/davidpangsw/aws-examples.git
```

- install aws cli
      cd /workspace
      # curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      # unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT

- authentication of aws client
    - Configuring environment variables for the AWS CLI
```sh
# use "gp env" instead of export, if your are using gitpod
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
export AWS_DEFAULT_REGION=us-west-2
aws sts get-caller-identity
# If you see your identity, success!
```


# S3
## Introduction
- Object-based Storage
    - *unlimited storage
- No need think about the underlying infrastructure
- S3 Console

- S3 Object
    - Like files
    - Consist of:
        - Key
        - Value
        - Version ID
        - Metadata
    - 0 to 5TB in size

- S3 Bucket
    - Hold objects / folders
    - S3 is a universal namespace, so bucket names must be unique (like having a domain name)