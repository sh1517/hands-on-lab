# Creating IAM User

### 1. IAM User 생성

- **IAM 메인 콘솔 화면 → 사용자 리소스 탭 → "사용자 생성" 버튼 클릭**

- 아래 정보 참고하여 설정

    - 이름: lab-edu-iam-user-01

    - 'AWS Management Console에 대한 사용자 액세스 권한 제공' 체크박스 활성화

    - 'IAM 사용자를 생성하고 싶음' 라디오 박스 활성화

    - '사용자 지정 암호' 라디오 박스 활성화 → 패스워드 지정

    - '사용자는 다음 로그인 시 새 암호를 생성해야 합니다 - 권장' 체크박스 해제
    
    - '다음' 버튼 클릭

        ![alt text](./img/iam_user_01.png)

    - '직접 정책 연결' 라디오 박스 활성화

    - 검색 창에 "AdministratorAccess" 입력 → "AdministratorAccess" 체크박스 활성

    - '다음' 버튼 클릭 → '사용자 생성' 버튼 클릭

        ![alt text](./img/iam_user_02.png)

### 2. AWS Account ID 정보 확인

- **콘솔 화면 우측 상단 계정이름 클릭 → 계정 ID 복사 버튼 클릭**

    ![alt text](./img/iam_user_03.png)

### 3. IAM User 이용 AWS Management Console 접속 테스트

- 브라우저에서 " ***Ctrl + Shift + n*** " 버튼 입력 (웹 브라우저 시크릿 모드 실행)

    ![alt text](./img/iam_user_04.png)

- **AWS 웹 사이트 → 로그인 화면 이동** ***(※ AWS Web URL: https://aws.amazon.com/ko)***

    ![alt text](./img/iam_user_05.png)

- 아래 정보 참고하여 입력

    - 계정 ID: 97********00 (자신의 계정 Account ID 값 입력)

    - 사용자 이름: lab-edu-iam-user-01

    - 암호: ***PASSWORD*** (자신이 설정한 패스워드 입력)

    - '로그인' 버튼 클릭

        ![alt text](./img/iam_user_06.png)

### 4. IAM User 권할 설정 테스트

- **IAM 메인 콘솔 화면 → 사용자 리소스 탭 → ***"lab-edu-iam-user-01"*** 클릭**

    ![alt text](./img/iam_user_policy_01.png)

- **'AdministratorAccess' 체크박스 활성화 → '제거' 버튼 클릭 → "정책 제거" 버튼 클릭**

    ![alt text](./img/iam_user_policy_02.png)

- EC2 메인 콘솔 화면으로 이동하여 리소스 정보 확인 ***(※ 권한이 없기 때문에 API 오류 발생)***

    ![alt text](./img/iam_user_policy_03.png)

- 웹 브라우저 '시크릿 모드' 창을 닫고 원래 브라우저 창에서 다음 작업 이어서 진행

<br><br>



# Creating Access key & Secret key

### 1. IAM User AdministratorAccess 권한 할당

- **IAM 메인 콘솔 화면 → 사용자 리소스 탭 → ***"lab-edu-iam-user-01"*** 클릭**

- '다음' 버튼 클릭 → '권한 추가' 버튼 클릭

    ![alt text](./img/access_secret_01.png)

- "ReadOnlyAccess" 검색 → 필터링 기준: "AWS 관리형 - 직무" 선택 → "ReadOnlyAccess" 권한 선택 → "다음" 버튼 클릭 → "권한 추가" 버튼 클릭

    ![alt text](./img/iam_setting_01.png)


### 2. Access key & Secret key 생성

- **'보안 자격 증명' 탭으로 이동 → '액세스 키 만들기' 버튼 클릭**

    ![alt text](./img/access_secret_02.png)

- 'Command Line Interface (CLI)' 라디오 박스 선택 → '위의 권장 사항을 이해했으며 액세스 키 생성을 계속하려고 합니다' 체크박스 활성화

- '다음' 버튼 클릭 → '액세스 키 만들기' 클릭 

    ![alt text](./img/access_secret_03.png)

- '표시' 버튼 클릭 → Access key & Secret key 메모장에 저장 ***(※ 해당 페이지 이탈 후 다시 확인이 불가능하기 때문에 별도 저장 필요)***

    ![alt text](./img/access_secret_04.png)

### 3. Access key & Secret key 활용

- Bastion 서버 접속

    - Putty 실행 → SSH 클릭 → Auth 클릭 → Credentials 클릭 → Browser 클릭 → 'lab-edu-key-ec2.ppk' 선택 

    - Session 클릭 → Host Name: 'ec2-user@*{BASTION_SERVER_PUBLIC_IP}* 입력 → 'Open' 버튼 클릭

- EC2 Bastion 서버에 Access & Secret key 설정

    ```bash
    $ aws configure
    AWS Access Key ID [None]: AKI**************GQD
    AWS Secret Access Key [None]: cLu************************************vlo
    Default region name [None]: ap-northeast-2
    Default output format [None]: json
    ```

- Access & Secret Key 적용 여부 확인

    ```bash
    $ aws sts get-caller-identity
    ```
    
    ```bash
    {
        "UserId": "AIDA6GBMEOUMBPJ5ZUVFO",
        "Account": "97********00",
        "Arn": "arn:aws:iam::97********00:user/lab-edu-iam-user-01"
    }
    ```

- Access & Secret Key에 할당된 권한 테스트

    - streamlit 서비스 실행

        ```bash
        sudo su -
        ```

        ```bash
        cd streamlit-project/
        ```
        
        ```bash
        streamlit run main.py --server.port 80
        ```

    - 브라우저에서 웹 서비스 접속 후 EC2 정보가 화면에 표시 되는지 확인

        ![alt text](./img/acces_key_test_01.png)
        ***※ EC2 Information 정보는 EC2 관련 IAM 권한이 없으면 접근 제한***

<br>

# Creating IAM Role

### 1. IAM Role 생성

- **IAM 메인 콘솔 화면 → 역할 리소스 탭 → "역할 생성" 버튼 클릭**

- 아래 정보 참고하여 설정

    - 신뢰할 수 있는 엔터티 유형: 'AWS 서비스' 라디오 박스 선택 

    - 사용 사례: EC2 → '다음' 버튼 클릭

        ![alt text](./img/iam_role_01.png)

    - 검색 창에 "AdministratorAccess" 입력 → "AdministratorAccess" 체크박스 활성 → '다음' 버튼 클릭

    - 역할 이름: lab-edu-role-ec2

    - '역할 생성' 버튼 클릭

### 2. EC2 instance profile 할당 (IAM Role → EC2 할당)

- **EC2 메인 콘솔 화면 → 인스턴스 리소스 탭 → "lab-edu-ec2-web" 선택 → 작업 → 보안 → IAM 역할 수정**

    ![alt text](./img/iam_role_02.png)

- IAM 역할: *lab-edu-role-ec2* 선택 → 'IAM 역할 업데이트' 버튼 클릭

    ![alt text](./img/iam_role_03.png)

- "lab-edu-ec2-web" 선택 → 인스턴스 상태 → '인스턴스 재부팅' 클릭

    ![alt text](./img/iam_role_04.png)

### 3. Web 서버 접속 및 streamlit 서비스 실행

- Bastion 서버 접속

    - Putty 실행 → SSH 클릭 → Auth 클릭 → Credentials 클릭 → Browser 클릭 → 'lab-edu-key-ec2.ppk' 선택 

    - Session 클릭 → Host Name: 'ec2-user@*{BASTION_SERVER_PUBLIC_IP}* 입력 → 'Open' 버튼 클릭

- EC2 접속 정보 확인: 인스턴스 메인 콘솔 화면 이동 → '인스턴스' 탭으로 이동 → 'lab-edu-ec2-web' 선택 → 프라이빗 IPv4 주소 복사

- Web 서버 접속

    - Bastion 서버를 접속 한 PuTTY 콘솔 화면에서 다음 명령어 실행

        ```bash
        ssh -i lab-edu-key-ec2.pem ec2-user@*{WEB_SERVER_PRIVATE_IP}*
        ```

- Web Service 실행 여부 확인 및 실행 명령어 입력

    - streamlit의 80번 포트 LISTEN 상태 여부 체크

        ```bash
        netstat -ntlp
        Active Internet connections (only servers)
        Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
        tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1500/sshd: /usr/sbi
        tcp6       0      0 :::22                   :::*                    LISTEN      1500/sshd: /usr/sbi
        ```
    
    - 80번 포트 LISTENING 상태가 아닌 경우 다음 명령어 실행

        ```bash
        sudo su -
        ```

        ```bash
        cd streamlit-project/
        ```

        ```bash
        streamlit run main.py --server.port 80
        ```

- 웹 서비스 접속 테스트 (로드밸런서 DNS 정보로 브라우저에서 접속)

    ![alt text](./img/iam_role_05.png)

