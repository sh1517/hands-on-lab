# Lab Environment Configuration

### 1. Route 53 Public Hosted Zone 구성

- **Route 53 콘솔 화면 → 호스트 영역 리소스 탭 → "호스팅 영역 생성" 버튼 클릭**

    ![alt text](./img/hosted_zone_01.png)

- 도메인 이름: **st _[개인 사물함 번호]_ .cj-cloud-wave.com** `(EX: st01.cj-cloud-wave.com)`

    ![alt text](./img/hosted_zone_02.png)

- Cloud9 IDE Terminal 화면으로 이동 
  
- Script 폴더로 이동

    ```bash
    ~/environment/cloud-wave-workspace/scripts
    ```

- Public Hosted Zone NS Record 정보 수집 Script 실행

    ```bash
    sh get_ns_record_json.sh [개인 사물함 번호]
    ```

- JSON_FILE 폴더 아래 JSON 파일 확인 → Slack으로 강사에게 공유

    ![alt text](./img/hosted_zone_03.png)

### 2. Web Service 실행

- Cloud9 IDE Terminal 화면으로 이동 

- Script 폴더로 이동

    ```bash
    ~/environment/cloud-wave-workspace/scripts
    ```

- Web Service *(Streamlit)* 설치 Script 실행

    ```bash
    sudo sh install_streamlit.sh
    ```

### 3. 서울, 버지니아, 프랑크프루트 Web Server 공인 IP 메모

- 인스턴스 메인 콘솔 화면 이동 → '인스턴스' 탭으로 이동 → 서울 리전으로 이동 → 'aws-cloud9-lab-edu-*' 선택 → 퍼블릭 IPv4 주소 복사 → 메모장 붙여 넣기

- 버지니아 리전으로 이동 → 'lab-edu-ec2-web-us' 선택 → 퍼블릭 IPv4 주소 복사 → 메모장 붙여 넣기

- 프랑크프루트 리전으로 이동 → 'lab-edu-ec2-web-eu' 선택 → 퍼블릭 IPv4 주소 복사 → 메모장 붙여 넣기

    ```
    ap-northeast-2  13.221.139.34
    us-east-1       3.34.56.125
    eu-central-1    42.24.48.126
    ```
<br><br>

# Route 53 Simple Routing Policy

# Route 53 Weighted Routing Policy

# Route 53 Latency Routing Policy

# Route 53 Failover Routing Policy

# Route 53 Geolocation Routing Policy