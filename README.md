### intro
This is a starter project to deploy apollo server into aws lambda
[Reference](https://medium.com/swlh/how-to-build-and-deploy-graphql-server-in-aws-lambda-using-nodejs-and-cloudformation-3e658cf9626f)

### steps
1. aws s3 mb s3://xz-lambda-apollo
2. zip -rq dist-latest.zip src package.json \
3. aws s3 cp dist-latest.zip s3://xz-lambda-apollo/dist-latest.zip

### cloudformation deployment
aws cloudformation deploy \
  --template-file ./cloudformation.yml \
  --stack-name xz-lambda-apollo-stack \
  --parameter-overrides BucketName=xz-lambda-apollo Version=latest \
  --capabilities CAPABILITY_IAM

### use sam for local
- [install](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
- sam local start-api --template-file cloudformation.yml

### validation
- zipinfo dist-latest.zip
- aws s3 ls s3://xz-lambda-apollo
- aws cloudformation describe-stacks --stack-name=xz-lambda-apollo-stack --query "Stacks[0].Outputs[?OutputKey=='ApiUrl'].OutputValue" --output text
- curl -d '{}' https://a3vhgxx5x4.execute-api.ap-southeast-2.amazonaws.com/v1/graphql
