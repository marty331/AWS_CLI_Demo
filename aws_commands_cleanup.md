[Home](./aws_commands.md)
## Clean-Up Commands

### Delete Lambda function

`aws lambda delete-function --function-name hello-cli`

### Delete IAM role

```bash
aws iam detach-role-policy \
  --role-name lambda-execution-role \
  --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
  ```

  `aws iam delete-role --role-name lambda-execution-role`

  ### Delete DynamoDB table

  `aws dynamodb delete-table --table-name DemoUsers`

### Delete S3 Files and Bucket

Delete file

`aws s3 rm s3://$BUCKET_NAME/test.txt`

Delete Bucket

`aws s3 rb s3://$BUCKET_NAME`
