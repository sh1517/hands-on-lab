- S3 Full Access 권한 할당

- Account ID 값 확인

    ```cmd
    aws sts get-caller-identity --query Account --output text
    ```

- S3 bucket 생성

    ```cmd
    aws s3 mb lab-edu-bucket-cf-repository-{ACCOUNT_ID}
    ```

- Data Upload to S3

    ```cmd
    aws s3 cp "Hands_on_Lab_07. CloudFormation\ap-northeast-2\01. vpc_resource.yaml" s3://lab-edu-bucket-cf-repository-975050143000/network-baseline.yaml
    ```


- AWS CLI 이용 CloudFormation Stack 생성

    ```cmd
    aws cloudformation create-stack \
    --stack-name lab-edu-cf-network-baseline-ap \
    --template-body file://"Hands_on_Lab_07. CloudFormation\ap-northeast-2\01. vpc_resource.yaml"
    ```