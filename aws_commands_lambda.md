[Home](./aws_commands.md)
## Lambda Function Demo

 Create IAM Role for Lambda

```bash
aws iam create-role --role-name lambda-execution-role \
  --assume-role-policy-document file://<(cat <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "lambda.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF
)
```

Attach permissions:

```bash
aws iam attach-role-policy \
  --role-name lambda-execution-role \
  --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

Wait ~30 seconds before proceeding (role propagation delay).

Create Lambda Function Code

`mkdir lambda_demo && cd lambda_demo`

```bash
cat > lambda_function.py <<EOF
def lambda_handler(event, context):
    name = event.get("name", "World")
    return {"message": f"Hello, {name}!"}
EOF
```

```bash
zip function.zip lambda_function.py
```

Deploy Lambda Function

```bash
aws lambda create-function \
  --function-name hello-cli \
  --runtime python3.12 \
  --zip-file fileb://function.zip \
  --handler lambda_function.lambda_handler \
  --role arn:aws:iam::<YOUR_ACCOUNT_ID>:role/lambda-execution-role
  ```
Replace <YOUR_ACCOUNT_ID> with your actual Account ID

Invoke the Lambda Function

```bash
aws lambda invoke \
  --function-name hello-cli \
  --payload $(echo "{\"name\":\"Postscript\"}" | base64) \
  response.json && cat response.json
```
🧾 Output: {"message": "Hello, Postscript!"}