# aws-cli-fu
Collection of my favorite AWS CLI calls, derived from `history` command and sanitized.

I'm using tmux with [aws-tmux](https://github.com/mhausenblas/aws-tmux) for the status and my global config is as follows:

```sh
$ cat ~/.aws/config
[default]
region = us-west-2
output = json
```

## General

```sh
aws configure set region eu-west-1
```

## Security

```sh
aws iam list-roles
aws iam list-users
aws iam list-user-tags --user-name k8s-greta
```

```sh
aws secretsmanager describe-secret --secret-id dev123.test
```

## Deploy

```sh
aws cloudformation create-stack --stack-name THESTACK --capabilities CAPABILITY_IAM --template-body file://amazon-eks-nodegroup-role.yaml --region us-west-2

aws cloudformation describe-stacks --stack-name THESTACK --query "Stacks[0].Outputs[?OutputKey=='TheAPIEndpoint'].OutputValue" --output text

aws cloudformation describe-stacks --stack-name THESTACK | jq -r '.Stacks[0].Outputs' | jq -c '.[] | select( .OutputKey == "SubnetIds" )' | jq -r '.OutputValue'

aws cloudformation describe-stacks --stack-name THESTACK | jq -r '.Stacks[0].Outputs' | jq -c '.[] | select( .OutputKey == "SubnetsPublic" )' | jq -r '.OutputValue' | sed "s/,/\\\,/g"

aws cloudformation wait stack-create-complete --stack-name THESTACK
```

## Storage

```sh
aws s3api create-bucket --bucket SOMEBUCKET --create-bucket-configuration LocationConstraint=$(aws configure get region) --region $(aws configure get region)

aws s3api get-bucket-tagging --bucket SOMEBUCKET

aws s3api get-object-tagging --bucket SOMEBUCKET --key SOME.FILE
```

```sh
aws dynamodb list-tags-of-resource --resource-arn arn:aws:dynamodb:us-west-2:123456789012:table/Music
```

```sh
aws rds describe-db-instances --db-instance-identifier mydb --region eu-west-1 | grep some

aws rds list-tags-for-resource --resource-name arn:aws:rds:eu-west-1:123456789012:db:mydb
```

```sh
aws sqs list-queue-tags --queue-url https://sqs.us-west-2.amazonaws.com/123456789012/myqueue
```

## Compute

```sh
aws lambda list-tags --resource arn:aws:lambda:us-west-2:123456789012:function:nasewebhook-PodsFunc-123456789
```

```sh
aws eks list-clusters

aws eks update-kubeconfig --name CLUSTERNAME

aws eks describe-cluster --name CLUSTERNAME | jq .cluster.resourcesVpcConfig.subnetIds

aws eks get-token --cluster-name CLUSTERNAME | jq -r '.status.token'

aws eks list-tags-for-resource --resource-arn arn:aws:eks:us-west-2:123456789102:cluster/CLUSTERNAME

aws eks delete-cluster --name CLUSTERNAME

```

```sh
aws ecs list-tags-for-resource --resource-arn arn:aws:ecs:us-west-2:123456789102:task-definition/nginx:1
```

```sh
aws ecr create-repository --region=us-west-2 --repository-name AREPO

aws ecr get-login --no-include-email

aws ecr list-tags-for-resource --resource-arn arn:aws:ecr:us-west-2:123456789012:repository/AREPO
```

```sh
aws ec2 describe-instances
```

## Networking

```sh
aws elbv2 describe-tags --resource-arns arn:aws:elasticloadbalancing:us-west-2:123456789012:loadbalancer/app/MYALB/123456789

aws elb describe-tags --load-balancer-names MYCLB
```

## Custom CLIs

There are couple of cases where I use, in addition to `aws` some custom CLIs. Some of them are maintained by ourselves, others by partners:

- For managing EKS clusters (creating, adding nodes, etc.) I'm using `eksctl`, see [eksctl.io](https://eksctl.io/)
- For managing Lambda apps, I'm using `sam`, see [awslabs/aws-sam-cli](https://github.com/awslabs/aws-sam-cli)
- For managing ECS clusters, I'm using  `ecs-cli`, see [aws/amazon-ecs-cli](https://github.com/aws/amazon-ecs-cli)


