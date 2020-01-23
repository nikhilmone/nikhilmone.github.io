# To use AWS SFTP, you take the following high-level steps:

- Create an Amazon S3 bucket, as described in Amazon S3 Bucket Requirements.

- Create an IAM role that contains two IAM policies:

An IAM policy that includes the permissions to enable AWS SFTP to access your S3 bucket. This IAM policy determines what level of access you provide your SFTP users.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::mybucket"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:DeleteObjectVersion",
                "s3:GetObjectVersion",
                "s3:GetObjectACL",
                "s3:PutObjectACL"
            ],
            "Resource": [
                "arn:aws:s3:::mybucket/*"
            ]
        }
    ]
}
```

An IAM policy to establish a trust relationship with AWS SFTP.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "transfer.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

- (Optional) If you have your own registered domain, associate your registered domain with the SFTP server.

You can route SFTP traffic to your SFTP server endpoint from a domain, such as example.com, or from a subdomain, such as sftp.accounting.example.com.

- Create an SFTP server and specify the identity provider type used by the service to authenticate your users.

- If you are working with an SFTP server with a service-managed identity provider, add one or more users.

- Open an SFTP client and configure the connection to use the SFTP endpoint host name for the SFTP server that you want to use. You can get this host name from the AWS SFTP Management Console.

- Create the user keys:

```
ssh-keygen -t rsa
```

- In User Configuration, select the role that you have created, Home directory as the S3 bucket and one of the folder as a root folder for FTP user. You can also restrict the folder, so that user can not access anything outside it, to use the same bucket for other users.

- Connect to the sftp server using command line, WinScp or FileZilla.

```
sftp -i sftp_id_rsa nikhil@1234567890.server.transfer.blah.amazonaws.com
Connected to 1234567890.server.transfer.blah.amazonaws.com
sftp> pwd
Remote working directory: /mybucket/input
sftp> cd ..
sftp> ls -al
drwxr--r--   1        -        -        0 Jan 23 08:05 input
drwxr--r--   1        -        -        0 Jan 23 08:05 output
```
