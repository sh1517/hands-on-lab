# Network Baseline 추가 (ap-northeast-2)

### 1. IAM User 리소스에 "S3 Full Access" 권한 할당

- **IAM 메인 콘솔 화면 → 사용자 리소스 탭 → "lab-edu-iam-user-01" 클릭**

- "권한 추가" 버튼 클릭 → "권한 추가" 버튼 클릭

- "직접 정책 연결" 라디오 버튼 선택 → "AmazonS3FullAccess" 검색 → "AmazonS3FullAccess" 권한 선택

### 2. CloudFormation YAML 파일 S3 업로드

- VS Code Terminal CMD 화면으로 이동

- Account ID 값 확인 및 환경변수 설정

    ```cmd
    aws sts get-caller-identity --query Account --output text
    ```

    ```cmd
    set ACCOUNT_ID=**********00
    ```

    ```cmd
    echo %ACCOUNT_ID%
    ```

- S3 bucket 생성

    ```cmd
    set BUCKET_NAME=lab-edu-bucket-cf-repository-%ACCOUNT_ID%
    ```

    ```cmd
    echo %BUCKET_NAME%
    ```

    ```cmd
    aws s3 mb s3://%BUCKET_NAME%
    ```

- Data Upload to S3

    ```cmd
    set FILE_PATH=Hands_on_Lab_07. CloudFormation\ap-northeast-2
    ```

    ```cmd
    set FILE_NAME=01. vpc_resource.yaml
    ```

    ```cmd
    set OBJ_NAME=network-baseline.yaml
    ```

    ```cmd
    aws s3 cp "%FILE_PATH%\%FILE_NAME%" s3://%BUCKET_NAME%/%OBJ_NAME%
    ```

### 3. YAML Template 이용 VPC 생성

- YAML Template 파일 구성 확인

    ![alt text](./img/template_01.png)

- 객체 URL 정보 생성
  
    ```cmd
    set OBJ_URL=https://%BUCKET_NAME%.s3.amazonaws.com/%OBJ_NAME%
    ```

    ```cmd
    echo %OBJ_URL%
    ```

- **CloudFormation 메인 콘솔 화면 → 스택 리소스 탭 → "스택 생성" 버튼 클릭**

    ![alt text](./img/template_02.png)

- 객체 URL 정보 입력 → '다음' 버튼 클릭

    ![alt text](./img/template_03.png)

- 스택 이름: lab-edu-cf-network-baseline-ap → '다음' 버튼 클릭 → '다음' 버튼 클릭 → '전' 버튼 클릭

    ![alt text](./img/template_04.png)




- AWS CLI 이용 CloudFormation Stack 생성

    ```cmd
    aws cloudformation create-stack \
    --stack-name lab-edu-cf-network-baseline-ap \
    --template-body file://"Hands_on_Lab_07. CloudFormation\ap-northeast-2\01. vpc_resource.yaml"
    ```