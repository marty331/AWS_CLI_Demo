[Home](./aws_commands.md)
## Check Current AWS Spend for the Month

Create variables for the dates - 

`TODAY=$(date +%Y-%m-%d)`

`START=$(date +%Y-%m-01)`

Get the current Cost and Usage data - 

`aws ce get-cost-and-usage --time-period Start=$START,End=$TODAY --granularity MONTHLY --metrics "UnblendedCost" --query "ResultsByTime[0].Total.UnblendedCost.Amount"`

Daily Spend over the past week -

```bash
aws ce get-cost-and-usage \
  --time-period Start=$(date -v-7d +%Y-%m-%d),End=$(date +%Y-%m-%d) \
  --granularity DAILY \
  --metrics "UnblendedCost" \
  --query 'ResultsByTime[*].{Date:Day,Cost:Total.UnblendedCost.Amount}'
```

Monthly Spend Grouped by Service

```bash
aws ce get-cost-and-usage \
  --time-period Start=$(date +%Y-%m-01),End=$(date +%Y-%m-%d) \
  --granularity MONTHLY \
  --metrics "UnblendedCost" \
  --group-by Type=DIMENSION,Key=SERVICE \
  --query 'ResultsByTime[0].Groups[*].{Service:Keys[0],Cost:Metrics.UnblendedCost.Amount}'
```

Forecasted Spend for the Month

```bash
aws ce get-cost-forecast \
  --time-period Start=$(date +%Y-%m-01),End=$(date -v+1m +%Y-%m-01) \
  --metric UNBLENDED_COST \
  --granularity MONTHLY
```

Usage Quantities Grouped by Service

```bash
aws ce get-cost-and-usage \
  --time-period Start=$(date +%Y-%m-01),End=$(date +%Y-%m-%d) \
  --granularity MONTHLY \
  --metrics "UsageQuantity" \
  --group-by Type=DIMENSION,Key=SERVICE \
  --query 'ResultsByTime[0].Groups[*].{Service:Keys[0],Usage:Metrics.UsageQuantity.Amount}'
```

### AWS CloudWatch Metrics

List All Available Metrics

`aws cloudwatch list-metrics`

You can filter by namespace, e.g., for Lambda:

`aws cloudwatch list-metrics --namespace AWS/Lambda`

Get Estimated Charges from CloudWatch

```bash
aws cloudwatch get-metric-statistics \
  --namespace AWS/Billing \
  --metric-name EstimatedCharges \
  --dimensions Name=Currency,Value=USD \
  --start-time $(date -u -v-1d +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 86400 \
  --statistics Maximum
```