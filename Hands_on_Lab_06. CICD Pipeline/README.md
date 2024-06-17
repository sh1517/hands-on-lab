# Creating CodeCommit

### 1. CodeCommit Repository 생성

- **CodeCommit 메인 콘솔 화면 → 리포지토리 리소스 탭 → "리포지토리 생성" 버튼 클릭**

    ![alt text](./img/code_commit_01.png)

- 아래 정보 참고하여 설정

    - 리포지토리 이름: lab-edu-code-streamlit

    - '생성' 버튼 클릭

        <img src="./img/code_commit_02.png" width="600" />

### 2. CodeCommit 전용 IAM User 생성

- **IAM 메인 콘솔 화면 → 사용자 리소스 탭 → "사용자 생성" 버튼 클릭**

- 아래 정보 참고하여 설정

    - 이름: lab-edu-iam-codecommit

    - 'AWS Management Console에 대한 사용자 액세스 권한 제공' 체크박스 활성화

    - 'IAM 사용자를 생성하고 싶음' 라디오 박스 활성화

    - '사용자 지정 암호' 라디오 박스 활성화 → 패스워드 지정

    - '사용자는 다음 로그인 시 새 암호를 생성해야 합니다 - 권장' 체크박스 해제
    
    - '다음' 버튼 클릭

<br>

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

- https://medium.com/@youngkeun.kim/aws-ci-cd-%EB%B0%B0%ED%8F%AC-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95-7fa70ba8bf78

- https://memory-hub.tistory.com/17

- https://dev.classmethod.jp/articles/create-aws-code-pipeline-ci-cd-environment-kr/

- https://www.youtube.com/watch?v=_o0GYyKnaI0

- https://github.com/wjdrbs96/Today-I-Learn




```
git init
git config --local init.defaultBranch main
git branch -M main
git remote add origin https://github.com/sh1517/streamlit-project.git

git add .
git commit -m "first commit"
git push origin main
```