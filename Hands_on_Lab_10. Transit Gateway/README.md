# Lab Environment Configuration

### 1. VPC Peering Resource 삭제

- **VPC 콘솔 메인 화면 → 피어링 연결 리소스 탭 → "lab-edu-peering-ap01-ap02" 선택**

- **'작업' 버튼 클릭 → '피어링 연결 삭제' 버튼 클릭 → '삭제' 입력 → '삭제' 버튼 클릭**

- **VPC 콘솔 메인 화면 → 피어링 연결 리소스 탭 → "lab-edu-peering-ap01-eu01" 선택**

- **'작업' 버튼 클릭 → '피어링 연결 삭제' 버튼 클릭 → '삭제' 입력 → '삭제' 버튼 클릭**
<br><br>



# 서울 리전 ↔ 서울 리전 Transit Gateway 생성

### 1. Transit Gateway 생성

- **VPC 콘솔 메인 화면 → Transit Gateway 리소스 탭 → "Transit Gateway 생성" 버튼 클릭**

- Transit Gateway 생성 정보 입력 → 'Transit Gateway 생성' 버튼 클릭

    - 이름: lab-edu-tgw-ap

    - Amazon ASN: 64512

        ![alt text](./img/transit_gateway_ap_ap_01.png)

### 2. Transit Gateway Attach 생성 (1/2)

- **VPC 콘솔 메인 화면 → Transit Gateway Attach 리소스 탭 → "Transit Gateway Attach 생성" 버튼 클릭**

- Transit Gateway Attach 생성 정보 입력 → 'Transit Gateway Attach 생성' 버튼 클릭

    - 이름: lab-edu-tgw-att-ap01

    - Transit Gateway ID: lab-edu-tgw-ap

    - 연결 유형: VPC

    - VPC ID: lab-edu-vpc-ap-01

    - Subnet ID: 

        - ap-northeast-2a: lab-edu-sub-tgw-01

        - ap-northeast-2c: lab-edu-sub-tgw-02

            ![alt text](./img/transit_gateway_ap_ap_02.png)
            
### 3. Transit Gateway Attach 생성 (2/2)

- **VPC 콘솔 메인 화면 → Transit Gateway Attach 리소스 탭 → "Transit Gateway Attach 생성" 버튼 클릭**

- Transit Gateway Attach 생성 정보 입력 → 'Transit Gateway Attach 생성' 버튼 클릭

    - 이름: lab-edu-tgw-att-ap02

    - Transit Gateway ID: lab-edu-tgw-ap

    - 연결 유형: VPC

    - VPC ID: lab-edu-vpc-ap-02

    - Subnet ID: 

        - ap-northeast-2a: lab-edu-sub-tgw-01

        - ap-northeast-2c: lab-edu-sub-tgw-02

### 4. Transit Gateway Routing Table 설정

- **VPC 콘솔 메인 화면 → Transit Gateway 라우팅 테이블 리소스 탭 → "Routing Table" 선택**

- '경로' 탭 → '정적 경로 생성' 버튼 클릭

    ![alt text](./img/transit_gateway_ap_ap_03.png)

- 정적 경로 생성 정보 입력 → '정적 경로 생성' 버튼 클릭

    - CIDR: 10.10.0.0/16

    - 연결 선택: lab-edu-tgw-att-ap02

### 5. Routing Table 수정

- **VPC 콘솔 메인 화면 → 라우팅 테이블 탭 → "lab-edu-rtb-pri-01" 선택 → '라우팅' 탭 → '라우팅 편집' 버튼 클릭**

- 라우팅 테이블 경로 생성 정보 입력

    - '라우팅 추가' 버튼 클릭

    - 대상: 10.10.0.0/16

    - 대상: Trangit Gateway (lab-edu-tgw-att-ap01)

    - '변경 사항 저장' 버튼 클릭

- **VPC 콘솔 메인 화면 → 라우팅 테이블 탭 → "lab-edu-rtb-2nd-pri-01" 선택 → '라우팅' 탭 → '라우팅 편집' 버튼 클릭**

- 라우팅 테이블 경로 생성 정보 입력

    - '라우팅 추가' 버튼 클릭

    - 대상: 10.0.0.0/16

    - 대상: Trangit Gateway (lab-edu-tgw-att-ap02)

    - '변경 사항 저장' 버튼 클릭

### 6. Network 통신 테스트

- **EC2 메인 콘솔 화면으로 이동 → 인스턴스 리소스 탭 → 'lab-edu-ec2-network-2nd-ap' 선택 → Private IP 주소 복사**

- Cloud9 IDE Terminal 화면으로 이동 → ssh 명령어 실행

    ```bash
    ssh web-server
    ```

- ICMP 통신 테스트 진행

    ```bash
    ping {2ND_VPC_NETWORK_SERVER_PRIVATE_IP}
    PING 10.10.40.140 (10.10.40.140) 56(84) bytes of data.
    64 bytes from 10.10.40.140: icmp_seq=1 ttl=254 time=0.990 ms
    64 bytes from 10.10.40.140: icmp_seq=2 ttl=254 time=0.926 ms
    64 bytes from 10.10.40.140: icmp_seq=3 ttl=254 time=0.907 ms
    64 bytes from 10.10.40.140: icmp_seq=4 ttl=254 time=0.946 ms
    ```




