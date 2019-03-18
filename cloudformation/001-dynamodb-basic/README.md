Deploy:
```shell
aws cloudformation deploy --template-file ./ddb.yml \
  --stack-name "test-$(basename $(pwd))" \
  --tags "Owner=Vincent Bourdaraud"
```

Destroy
```shell
aws cloudformation delete-stack --stack-name "test-$(basename $(pwd))"
```
