Contents
====================================================
  * [Setup AWS CLI](#aws-cli)
  * [AWS accounts](#aws-accounts)
  * [File upload/processing on S3](#aws-s3-upload)

<a name="aws-cli">Setup AWS CLI</a>
----------------------------------------------------
First thing to do when setting up AWS on your machine is to 
set up authentication. For that you need to run command
```console
aws configure
AWS Access Key ID [None]: (provided by eliptica-developers account root)
AWS Secret Access Key [None]: (provided by eliptica-developers account root)
Default region name [None]: eu-west-2
Default output format [None]: table
```

This will create two files `~/.aws/config` and `~/.aws/credentials`. 
If you are using multiple users you can add `[another-user]` directive and use
`aws --profile another-user` to execute any aws command not under other than default profile

For developers it is strongly recommended to create `[developer]` profile as this profile 
will be used in all AWS configuration scripts in the feature

**~/.aws/credentials**
```ini
[default]
aws_access_key_id = XXXXXXXXXXXXXXXXXXXX
aws_secret_access_key = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
[developer]
aws_access_key_id = XXXXXXXXXXXXXXXXXXXX
aws_secret_access_key = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

**~/.aws/credentials**
```ini
[default]
region = eu-west-2
output = table
[developer]
region = eu-west-2
output = tables
```

<a name="aws-accounts">AWS accounts</a>
----------------------------------------------------
  * `eliptica-developers` [console](https://eliptica-developers.signin.aws.amazon.com/console)
  * `eliptica-devops` [console](https://eliptica-devops.signin.aws.amazon.com/console)


<a name="#aws-s3-upload">File upload/processing on S3</a>
----------------------------------------------------
> <img alt="Warning" src="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/light-theme/warning.svg">
> <br>
> One important thing is that this example uses S3 buckets but the cheaper space is called S3 glacier vault. Need to check whether the approach and syntax is the same!

This section explains step-by-step how to upload a file or multiple files to S3

 1. Synchronize system time
```bash
sudo apt-get install ntpdate
sudo ntpdate time.nist.gov
```
 2. Create S3 bucket in the root account (Disable ACL)
 3. Create policy that allows read-only access to S3 bucket (for raw upload) and read/write for processing raw files and writing to S3. Use [this](https://stackoverflow.com/questions/12700921/s3-moving-files-between-buckets-on-different-accounts/17162973#17162973) article to create a separate user to process raw data

**eliptica-proteomics-raw-upload policy**
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PolicyUniqueID1",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::eliptica-proteomics-raw/*"
        }
    ]
}
```
**eliptica-proteomics-raw-process policy**
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PolicyUniqueID1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::eliptica-proteomics-raw/*"
        },
        {
            "Sid": "PolicyUniqueID1",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::eliptica-proteomics-processed/*"
        }
      
    ]
}
```

 * Create user for accessing the buckets in the same account for accessing data (Web console)
 * Upload file
```bash
aws s3 --profile developer cp path/to/file.ext s3://eliptica-proteomics-raw/file.ext
```
