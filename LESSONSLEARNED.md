Lessons Learned

* CloudFormation
  * Can't be used alone for IaC.
    * Needs CLI to workaround missing features (ex: DynamoDB GlobalTables)
    * StackSets requires CLI and are cumbersome to use (way too many steps)
    * Terraform does a much better job here
