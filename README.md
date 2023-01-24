Contents
====================================================
  * [Setup AWS CLI](#aws-cli)
  * [AWS accounts](#aws-accounts)

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
