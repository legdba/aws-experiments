Create a StackSet
```shell
aws cloudformation create-stack-set \
  --stack-set-name "test-$(basename $(pwd))" \
  --template-body file://./ddb.yml
```

Instantiate stacks in `us-east-1` and `us-west-2`
```shell
aws cloudformation create-stack-instances \
  --stack-set-name "test-$(basename $(pwd))" \
  --regions "us-east-1" "us-west-2 \
  --account "$(aws sts get-caller-identity \
  --output text --query 'Account')"
```

Setup Global Table (not supported by CloudFormation as of March 2019).
```shell
aws dynamodb create-global-table \
  --global-table-name Counters \
  --replication-group RegionName=us-east-1 RegionName=us-west-2 \
  --region us-east-1
```

Push some objects for testing
```shell
aws dynamodb put-item \
  --table-name Counters \
  --item '{"CounterId": {"S":"001"},"CounterValue": {"N":"1"}}' \
  --region us-west-2
```

Get them back (note: wait a bit for the replication)
```shell
aws dynamodb get-item \
  --table-name Counters \
  --key '{"CounterId": {"S":"001"}}' \
  --region us-east-1
```

Destroy all instances
```shell
aws cloudformation delete-stack-instances \
  --stack-set-name "test-$(basename $(pwd))" \
  --regions "us-east-1" "us-west-2" \
  --account "$(aws sts get-caller-identity \
  --output text --query 'Account')" \
  --no-retain-stacks
```

And finally delete the StackSet
```shell
aws cloudformation delete-stack-set \
  --stack-set-name "test-$(basename $(pwd))"
```

