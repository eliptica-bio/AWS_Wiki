<a name="contents">Contents</a>
====================================================
  * [Install AWS CLI](#aws-cli)
  * [Upload data to S3](#aws-accounts)
  * [Uninstall AWS CLI](#aws-uninstall)
  * [Eliptica preparation steps](#aws-eliptica)

<a name="aws-cli">Install AWS CLI</a>
----------------------------------------------------
Install AWS CLI into __$AWS_PATH__

```bash
export AWS_PATH=~/eliptica-awscliv2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o ${AWS_PATH}.zip
unzip ${AWS_PATH}.zip -d ${AWS_PATH} && rm ${AWS_PATH}.zip
${AWS_PATH}/aws/install -b ${AWS_PATH} -i ${AWS_PATH}
${AWS_PATH}/v2/current/bin/aws --version
```

<a name="aws-sync">Upload data to S3</a>
----------------------------------------------------

```bash
export AWS_ACCESS_KEY_ID="AWS_ACCESS_KEY_ID" 
export AWS_SECRET_ACCESS_KEY="AWS_SECRET_ACCESS_KEY" 
export AWS_DEFAULT_REGION=eu-west-2 
${AWS_PATH}/v2/current/bin/aws s3 sync /path/to/gs_data s3://eliptica-gs-data/
```

<a name="aws-uninstall">Uninstall AWS CLI</a>
----------------------------------------------------
```bash
rm -Rf ${AWS_PATH}
```


<a name="aws-eliptica">Eliptica preparation steps</a>
----------------------------------------------------
On our part we need to
 * Create a temporary user 
   * [gs_data_upload](https://eliptica-aws-production.signin.aws.amazon.com/console)
 * Create a S3 bucket 
   * `s3://eliptica-gs-data`
   * ObjectLock is enabled but we need to lock the data after upload
 * Create AWS cli synchronization policy
   * Policy `Policy_S3_Sync_eliptica-gs-data`
   * Group `Group_S3_Sync_eliptica-gs-data`
   * The policy contains `s3:DeleteObject` premissions which only works if `aws s3 sync --delete` option is enabled. And will be overwritten once the ObjectLock is enabled

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::eliptica-gs-data"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetBucketLocation",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::eliptica-gs-data/*"
        }
    ]
}
```



