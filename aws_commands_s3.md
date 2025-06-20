[Home](./aws_commands.md)
## S3 Bucket Operations

 Create a variable for the bucket name - 

`BUCKET_NAME="demo-cli-bucket-$RANDOM"`

Create the S3 bucket -

`aws s3 mb s3://$BUCKET_NAME`

Create a sample file

`echo "This is a test file created on $(date)" > test.txt`

Upload the file to the S3 Bucket

`aws s3 cp test.txt s3://$BUCKET_NAME/`

List the contents of the bucket

`aws s3 ls s3://$BUCKET_NAME/`

Download the file back from S3 - 

`aws s3 cp s3://$BUCKET_NAME/test.txt downloaded_test.txt`

Test the new file contents - 

`cat downloaded_test.txt`
