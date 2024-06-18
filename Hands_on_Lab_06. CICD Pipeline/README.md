# Creating CodeCommit

### 1. CodeCommit Repository 생성

- **CodeCommit 메인 콘솔 화면 → 리포지토리 리소스 탭 → "리포지토리 생성" 버튼 클릭**

    ![alt text](./img/code_commit_01.png)

- 아래 정보 참고하여 설정

    - 리포지토리 이름: lab-edu-code-streamlit

    - '생성' 버튼 클릭

        <img src="./img/code_commit_02.png" width="600" />


### 2. Local Workspace 구성

- **Git 다운로드 URL:** https://git-scm.com/downloads

- **VS Code 다운로드 URL:** https://code.visualstudio.com/download

- **Extensions 설치:** python, AWS CLI Configure

    ![alt text](./img/vs_code_setting_01.png)

- **실습 Code 다운로드 URL:** https://github.com/sh1517/streamlit-project/tree/main

- "Code" 버튼 클릭 → "Download ZIP" 버튼 클릭 → 다운로드 받은 파일 바탕화면으로 이동 → 압축 풀기

    ![alt text](./img/vs_code_setting_02.png)


### 3. Access & Secret key Setting (VS Code)

- **IAM 메인 콘솔 화면 → 사용자 리소스 탭 → "lab-edu-iam-user-01" 클릭**

- "권한 추가" 버튼 클릭 → "권한 추가" 버튼 클릭

- "직접 정책 연결" 라디오 버튼 선택 → "AWSCodeCommitPowerUser" 검색 → "AWSCodeCommitPowerUser" 권한 선택

    ![alt text](./img/iam_setting_01.png)

- VS Code Terminal CMD 화면으로 이동

    ![alt text](./img/iam_setting_03.png)

- VS Code Terminal에서 Access & Secret key 설정

    ```bash
    > aws configure
    AWS Access Key ID [None]: AKI**************GQD
    AWS Secret Access Key [None]: cLu************************************vlo
    Default region name [None]: ap-northeast-2
    Default output format [None]: json
    ```


### 4. VS Code → CodeCommit SSH Push 테스트

- VS Code에서 소스 코드 파일 오픈

    ![alt text](./img/push_01.png)

- 탐색기: 바탕화면 → "streamlit-project-main" 폴더 접속 → "streamlit-project-main" 폴더 클릭(두 번째 폴더) → "폴더 선택" 버튼 클릭

    ![alt text](./img/push_02.png)

- VS Code Terminal CMD 화면으로 이동

- SSH RSA Key 생성

    ```bash
    > ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (C:\Users\sh****.you/.ssh/id_rsa): #Enter
    Enter passphrase (empty for no passphrase): #Enter
    Enter same passphrase again: #Enter
    ```
- 홈 디렉터리의 ".ssh" 폴더로 이동 (C:\Users\sh****.you/.ssh/) → "id_rsa.pub" 파일 메모장으로 실행

    ![alt text](./img/push_03.png)

- 전체 내용 복사 *(예시)*

    ```bash
    ssh-rsa EXAMPLE-AfICCQD6m7oRw0uXOjANBgkqhkiG9w0BAQUFADCBiDELMAkGA1UEBhMCVVMxCzAJB
    gNVBAgTAldBMRAwDgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDASBgNVBAsTC0lBTSBDb2
    5zb2xlMRIwEAYDVQQDEwlUZXN0Q2lsYWMxHzAdBgkqhkiG9w0BCQEWEG5vb25lQGFtYXpvbi5jb20wHhc
    NMTEwNDI1MjA0NTIxWhcNMTIwNDI0MjA0NTIxWjCBiDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAw
    DgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDAS=EXAMPLE user-name@computer-name
    ```

- **IAM 메인 콘솔 화면 → 사용자 리소스 탭 → "lab-edu-iam-user-01" 클릭 → 보안자격증명 탭**

    ![alt text](./img/push_04.png)

- "SSH 퍼블릭 키 업로드" 버튼 클릭

    ![alt text](./img/push_05.png)

- "SSH 키 ID" 복사

    ![alt text](./img/push_06.png)

- 홈 디렉터리의 ".ssh" 폴더로 이동 (C:\Users\sh****.you/.ssh/) → "config" 생성 후 메모장으로 실행

    ![alt text](./img/push_07.png)

- 아래 코드 입력 후 개인 SSH 키 ID 값으로 치환

    ```bash
    Host git-codecommit.*.amazonaws.com
    User A******************Y   # 개인별로 생성된 SSH 키 ID 값으로 치환
    IdentityFile ~/.ssh/codecommit
    ```

- VS Code Terminal CMD 화면으로 이동 → SSH 설정 테스트

    ```cmd
    ssh git-codecommit.us-east-2.amazonaws.com
    ```

- Local Git Repository 초기화

    ```cmd
    git init
    ```

- Local Branch name setting (main)

    ```cmd
    git branch -M main
    ```

- **CodeCommit 메인 콘솔 화면 → 리포지토리 리소스 탭 → "lab-edu-code-streamlit" 클릭 → "URL 복제" 버튼 클릭 → "SSH 복제" 버튼 클릭**

    ![alt text](./img/push_09.png)

- Remote Repository 등록

    ```cmd
    git remote add ssh://git-codecommit.ap-northeast-2.amazonaws.com/v1/repos/lab-edu-code-streamlit
    ```

- 소스코드 Commit 후 Push 테스트

    ```cmd
    git add .
    git commit -m "first commit"
    git push origin main
    ```

- AWS CodeCommit 레포지토리 콘솔 확인

    ![alt text](./img/push_08.png)

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
