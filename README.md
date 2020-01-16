# aws-cli-fu
Collection of my favorite AWS CLI calls

```sh
aws configure set region eu-west-1

aws s3api create-bucket --bucket SOMEBUCKET --create-bucket-configuration LocationConstraint=$(aws configure get region) --region $(aws configure get region)
aws s3api get-bucket-tagging --bucket SOMEBUCKET
aws s3api get-object-tagging --bucket SOMEBUCKET --key SOME.FILE

aws ec2 describe-instances

aws eks --region eu-west-1 update-kubeconfig --name fgconsole
aws eks describe-cluster --name mngbase | jq .cluster.resourcesVpcConfig.subnetIds
aws eks get-token --cluster-name eksworkshop-eksctl | jq -r '.status.token'
aws eks list-tags-for-resource --resource-arn arn:aws:eks:us-west-2:148658015984:cluster/mngbase
aws eks delete-cluster --name eks-1-14-hausenbl-11-07-2019 --endpoint-url https://api.beta.us-west-2.wesley.amazonaws.com
aws eks list-clusters

aws cloudformation describe-stacks --stack-name THESTACK --query "Stacks[0].Outputs[?OutputKey=='TheAPIEndpoint'].OutputValue" --output text
aws cloudformation describe-stacks --stack-name THESTACK | jq -r '.Stacks[0].Outputs' | jq -c '.[] | select( .OutputKey == "SubnetIds" )' | jq -r '.OutputValue'
aws cloudformation describe-stacks --stack-name THESTACK | jq -r '.Stacks[0].Outputs' | jq -c '.[] | select( .OutputKey == "SubnetIds" )' | jq -r '.OutputValue'
aws cloudformation describe-stacks --stack-name THESTACK | jq -r '.Stacks[0].Outputs' | jq -c '.[] | select( .OutputKey == "SubnetsPublic" )' | jq -r '.OutputValue' | sed "s/,/\\\,/g"
aws cloudformation create-stack --stack-name THESTACK --capabilities CAPABILITY_IAM --template-body file://amazon-eks-nodegroup-role.yaml --region us-west-2
aws cloudformation wait stack-create-complete --stack-name THESTACK --region us-west-2

aws iam list-roles
aws iam list-users
aws iam list-user-tags --user-name k8s-greta

aws ecr list-tags-for-resource --resource-arn arn:aws:ecr:us-west-2:123456789012:repository/secureping

aws ecs list-tags-for-resource --resource-arn arn:aws:ecs:us-west-2:123456789102:task-definition/nginx:1

aws lambda list-tags-for-resource --resource-arn arn:aws:lambda:us-west-2:123456789012:function:nasewebhook-PodsFunc-123456789
aws lambda list-tags --resource-arn arn:aws:lambda:us-west-2:123456789012:function:nasewebhook-PodsFunc-123456789
aws lambda list-tags --resource arn:aws:lambda:us-west-2:123456789012:function:nasewebhook-PodsFunc-123456789

aws dynamodb list-tags-of-resource --resource-arn arn:aws:dynamodb:us-west-2:123456789012:table/Music

aws elbv2 describe-tags --resource-arns arn:aws:elasticloadbalancing:us-west-2:123456789012:loadbalancer/app/my-test-alb/123456789
aws elb describe-tags --load-balancer-names my-test-clb

aws secretsmanager describe-secret --secret-id dev123.test

aws rds describe-db-instances --db-instance-identifier mydb --region eu-west-1 | grep some
aws rds list-tags-for-resource --resource-name arn:aws:rds:eu-west-1:123456789012:db:mydb
aws rds list-tags-for-resource --resource-name arn:aws:rds:eu-west-1:123456789012:db:mydb --region eu-west-1

aws sqs list-queue-tags --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/myqueue

aws ecr create-repository --region=us-west-2 --repository-name AREPO
aws ecr get-login --no-include-email
```
