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

### 3. 서울, 버지니아, 프랑크프루트 웹 서버 Public IP 메모

- 인스턴스 메인 콘솔 화면 이동 → '인스턴스' 탭으로 이동 → 서울 리전으로 이동 → 'aws-cloud9-lab-edu-*' 선택 → 퍼블릭 IPv4 주소 복사 → 메모장 붙여 넣기

- 버지니아 리전으로 이동 → 'lab-edu-ec2-web-us' 선택 → 퍼블릭 IPv4 주소 복사 → 메모장 붙여 넣기

- 프랑크프루트 리전으로 이동 → 'lab-edu-ec2-web-eu' 선택 → 퍼블릭 IPv4 주소 복사 → 메모장 붙여 넣기

    ```
    ap-northeast-2  3.34.197.127
    us-east-1       44.208.22.154
    eu-central-1    54.93.237.6
    ```
<br><br>




# Route 53 Simple Routing Policy

### 1. Single Value Simple Routing Policy 생성

- **Route 53 메인 콘솔 화면 → 호스팅 영역 리소스 탭 → "stxx.cj-cloud-wave.com" 클릭**

    ![alt text](./img/simple_policy_01.png)

- '레코드 생성' 버튼 클릭

    ![alt text](./img/simple_policy_02.png)

- Routing Policy 생성 정보 입력

    - 레코드 이름: ***<span style="color:orange">simple</span>.domain-name.com***

    - 레코드 유형: ***A***

    - 값: ***<span style="color:orange">ap-northeast-2</span> web server public ip***

    - TTL(option): ***10초***

    ![alt text](./img/simple_policy_03.png)

### 2. 웹 서비스 접속 테스트 (http://simple.stxx.cj-cloud-wave.com/ 접속)
  
![alt text](./img/simple_policy_04.png)

### 3. Multy Value Simple Routing Policy 생성

- Route 53 레코드 편집 화면으로 이동 → 'simple.domain-name.com' 레코드 클릭 → '레코드 편집' 버튼 클릭

    ![alt text](./img/simple_policy_05.png)

- 값 수정 → '저장' 버튼 클릭
  
    - ***<span style="color:orange">ap-northeast-2</span> web server public ip***

    - ***<span style="color:orange">us-east-1</span> web server public ip***

    - ***<span style="color:orange">eu-central-1</span> web server public ip***

        ![alt text](./img/simple_policy_06.png)

### 4. Linux 'dig' 명령어 이용 DNS record 값 반영 확인

- Cloud9 IDE Terminal 화면 이동 → dig 명령 입력

    ```bash
    hands-on:~/environment $ dig simple.cj-cloud-wave.com

    ; <<>> DiG 9.16.48-RH <<>> simple.cj-cloud-wave.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42468
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4096
    ;; QUESTION SECTION:
    ;simple.cj-cloud-wave.com.      IN      A

    ;; ANSWER SECTION:
    simple.cj-cloud-wave.com. 1     IN      A       54.93.237.6
    simple.cj-cloud-wave.com. 1     IN      A       3.34.197.127
    simple.cj-cloud-wave.com. 1     IN      A       44.208.22.154

    ;; Query time: 70 msec
    ;; SERVER: 10.0.0.2#53(10.0.0.2)
    ;; WHEN: Sat Jun 29 00:02:45 UTC 2024
    ;; MSG SIZE  rcvd: 101
    ```
<br><br>





# Route 53 Weighted Routing Policy

### 1. Weighted Routing Policy 생성 

- **Route 53 메인 콘솔 화면 → 호스팅 영역 리소스 탭 → "stxx.cj-cloud-wave.com" 클릭**

- '레코드 생성' 버튼 클릭

- Routing Policy 생성 정보 입력 (서울 리전) → '다른 레코드 추가' 버튼 클릭

    - 레코드 이름: ***<span style="color:orange">weighted</span>.domain-name.com***

    - 레코드 유형: ***A***

    - 값: ***<span style="color:orange">ap-northeast-2</span> web server public ip***

    - TTL(option): ***1초***

    - Routing Policy: 가중치 기반

    - Weight: 60

    - Record id: ap-northeast-2

    ![alt text](./img/simple_policy_07.png)

- Routing Policy 생성 정보 입력 (버지니아 리전) → '다른 레코드 추가' 버튼 클릭

    - 레코드 이름: ***<span style="color:orange">weighted</span>.domain-name.com***

    - 레코드 유형: ***A***

    - 값: ***<span style="color:orange">us-east-1</span> web server public ip***

    - TTL(option): ***1초***

    - Routing Policy: 가중치 기반

    - Weight: 30

    - Record id: us-east-1

- Routing Policy 생성 정보 입력 (프랑크프루트 리전) → '레코드 생성' 버튼 클릭

    - 레코드 이름: ***<span style="color:orange">weighted</span>.domain-name.com***

    - 레코드 유형: ***A***

    - 값: ***<span style="color:orange">eu-central-1</span> web server public ip***

    - TTL(option): ***1초***

    - Routing Policy: 가중치 기반

    - Weight: 10

    - Record id: eu-central-1

### 2. Linux 'dig' 명령어 이용 DNS record 값 반영 확인

- Cloud9 IDE Terminal 화면 이동 → dig 명령 입력 (수 차례 시도 시 가중치 기반 ANSWER SECTION 값 변경)

    ```bash
    # 첫 번째 도메인 주소 검색 시도
    hands-on:~/environment $ dig weighted.cj-cloud-wave.com

    ; <<>> DiG 9.16.48-RH <<>> weighted.cj-cloud-wave.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 7098
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4096
    ;; QUESTION SECTION:
    ;weighted.cj-cloud-wave.com.    IN      A

    ;; ANSWER SECTION:
    weighted.cj-cloud-wave.com. 1   IN      A       3.34.197.127

    ;; Query time: 0 msec
    ;; SERVER: 10.0.0.2#53(10.0.0.2)
    ;; WHEN: Sat Jun 29 00:18:36 UTC 2024
    ;; MSG SIZE  rcvd: 71

    # 두 번째 도메인 주소 검색 시도
    hands-on:~/environment $ dig weighted.cj-cloud-wave.com

    ; <<>> DiG 9.16.48-RH <<>> weighted.cj-cloud-wave.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4630
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4096
    ;; QUESTION SECTION:
    ;weighted.cj-cloud-wave.com.    IN      A

    ;; ANSWER SECTION:
    weighted.cj-cloud-wave.com. 1   IN      A       44.208.22.154

    ;; Query time: 30 msec
    ;; SERVER: 10.0.0.2#53(10.0.0.2)
    ;; WHEN: Sat Jun 29 00:18:39 UTC 2024
    ;; MSG SIZE  rcvd: 71
    ```







# Route 53 Latency Routing Policy

# Route 53 Failover Routing Policy

# Route 53 Geolocation Routing Policy