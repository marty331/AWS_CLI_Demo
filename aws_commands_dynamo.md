[Home](./aws_commands.md)
## DynamoDB Table Demo

 Create Table

```bash
aws dynamodb create-table \
  --table-name DemoUsers \
  --attribute-definitions AttributeName=UserId,AttributeType=S \
  --key-schema AttributeName=UserId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST
```

Insert Data

```
aws dynamodb put-item \
  --table-name DemoUsers \
  --item '{"UserId": {"S": "u123"}, "Name": {"S": "Marty"}, "Role": {"S": "Engineer"}}'
```

Get Data

```bash
aws dynamodb get-item \
  --table-name DemoUsers \
  --key '{"UserId": {"S": "u123"}}'
```