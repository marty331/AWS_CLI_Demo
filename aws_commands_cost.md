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
  --query 'ResultsByTime[*].{Date:TimePeriod.Start,Cost:Total.UnblendedCost.Amount}'
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

Usage Quantities Grouped by Service
Shows Monthly usage data grouped by AWS service, showing the total amount of usage (not cost) for each service
used from the start of the current month to today.

```bash
aws ce get-cost-and-usage \
  --time-period Start=$(date +%Y-%m-01),End=$(date +%Y-%m-%d) \
  --granularity MONTHLY \
  --metrics "UsageQuantity" \
  --group-by Type=DIMENSION,Key=SERVICE \
  --query 'ResultsByTime[0].Groups[*].{Service:Keys[0],Usage:Metrics.UsageQuantity.Amount}'
```
