Crick should only run the following command
```bash
AWS_ACCESS_KEY_ID="AWS_ACCESS_KEY_ID" AWS_SECRET_ACCESS_KEY="AWS_SECRET_ACCESS_KEY" AWS_DEFAULT_REGION=eu-west-2 aws s3 sync ~/Workspace/aws_sync s3://eliptica-proteomics-raw/
```


On our part we need to 
 * create a temporary user (save __key__ and __secret__)
 * create a bucket 
 * create Sync policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::eliptica-proteomics-raw"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetBucketLocation",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::eliptica-proteomics-raw/*"
        }
    ]
}
```



