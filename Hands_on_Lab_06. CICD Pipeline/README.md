# Creating CodeCommit

### 1. IAM User 생성

- **IAM 메인 콘솔 화면 → 사용자 리소스 탭 → "사용자 생성" 버튼 클릭**


# Creating CodeDeploy

### 1. CodeDeploy Agent 설치

- 관련 패키지 다운로드

    ```bash
    sudo yum update -y && sudo yum install ruby -y && sudo yum install wget -y
    ```

- CodeDeploy Agent 설치 프로그램 다운로드

    ```bash
    wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install
    ```

- 설치 파일 실행 권한 할당

    ```bash
    chmod +x ./install
    ```

- 설치 파일 이용 최신 버전 설치 

    ```bash
    sudo ./install auto
    ```

- CodeDeploy Agent 서비스 실행 및 상태 확인 명령

    ```bash
    systemctl status codedeploy-agent   # sudo service codedeploy-agent status
    systemctl start codedeploy-agent    # sudo service codedeploy-agent start
    ```

# Creating CodePipeline


# Reference Documents

- AWS CodeDeploy: https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations-create.html