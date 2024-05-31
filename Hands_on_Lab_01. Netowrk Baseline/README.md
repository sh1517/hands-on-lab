# Creating Network Baseline
**자원 명세서:** 서울 리전 네트워크 리소스 생성 정보
|Region|VPC_Name|CIDR|Subnet_Name|CIDR|Routing_Table_Name|
|:---:|:---:|:---:|:---:|:---:|:---:|
|ap-northeast-2|lab-edu-vpc-ap-01|10.0.0.0/16 |lab-edu-sub-pub-01|10.0.0.0/24 |lab-edu-rtb-pub    |
|   |   |                                     |lab-edu-sub-pub-02|10.0.1.0/24 |lab-edu-rtb-pub    |
|   |   |                                     |lab-edu-sub-pri-01|10.0.40.0/24|lab-edu-rtb-pri-01 |
|   |   |                                     |lab-edu-sub-pri-02|10.0.41.0/24|lab-edu-rtb-pri-02 |

**라우팅 테이블 경로 설정**
|Table_Name|Destination|Target|
|:---:|:---:|:---:|
|lab-edu-rtb-pub      |10.0.0.0/16   |Local|
|                     |0.0.0.0/0     |lab-edu-igw-ap (igw id)|
|lab-edu-rtb-pri-01   |10.0.0.0/16   |Local|
|                     |0.0.0.0/0     |lab-edu-natgw-ap-01 (natgw id)|
|lab-edu-rtb-pri-02   |10.0.0.0/16   |Local|



## Create VPC
### 1. VPC 메인 콘솔 화면으로 이동
![image](./img/vpc_01.png)

### 2. VPC 리소스 탭으로 이동
![image](./img/vpc_02.png)

### 3. 'VPC 생성' 버튼 클릭
![image](./img/vpc_03.png)

### 4. VPC 생성 정보 입력
- **생성할 리소스:** VPC만
- **이름 태그:** lab-edu-vpc-ap-01
- **IPv4 CIDR:** 10.0.0.0/16

<img src="./img/vpc_04.png" width="600" />




## Create Subnet
### 1. Subnet 리소스 탭으로 이동
![image](./img/subnet_01.png)

### 2. "Subnet 생성" 버튼 클릭
![image](./img/subnet_02.png)

### 3. Subnet 생성 정보 입력
- 아래 서브넷 자원 명세서를 참고하여 정보 입력
- 화면 하단의 '서브넷 추가' 버튼 이용 여러 개의 서브넷 정보 동시 입력 가능 
|  |Public Subnet 01|Public Subnet 02|Private Subnet 01|Private Subnet 02|
|:---:|:---:|:---:|:---:|:---:|
|VPC_ID|leb-edu-vpc-ap-01|leb-edu-vpc-ap-01|leb-edu-vpc-ap-01|leb-edu-vpc-ap-01|
|Subnet_Name|lab-edu-sub-pub-01|lab-edu-sub-pub-02|lab-edu-sub-pri-01|lab-edu-sub-pri-02|
|Availability_Zone|ap-northeast-2a|ap-northeast-2c|ap-northeast-2a|ap-northeast-2c|
|IPv4 CIDR|10.0.0.0/24|10.0.1.0/24|10.0.40.0/24|10.0.41.0/24|
<img src="./img/subnet_03.png" width="600" />

### 4. Public Subnet 추가 설정
- **Subnet 리소스 탭 → lab-edu-sub-pub-01 선택 → '작업' 버튼 클릭 → '서브넷 설정 편집' 버튼 클릭 → '퍼블릭 IPv4 주소 지정 할당' 활성화 → 저장**
- **lab-edu-sub-pub-02 서브넷도 동일하게 설정**

<img src="./img/subnet_04.png" width="800" />



## Internet Gateway 생성
### 1. Internet Gateway 리소스 탭으로 이동
- **'인터넷 게이트웨이' 리소스 탭 → '인터넷 게이트웨이 생성' 버튼 클릭**

![image](./img/route_01.png)

### 2. Internet Gateway 생성 정보 입력
- 인터넷 게이트웨이 이름: lab-edu-igw-ap
- VPC: lab-edu-vpc-ap-01

![image](./img/route_02.png)

### 3. Internet Gateway → VPC 할당
- **Internet Gateway 세부 정보 화면 → 우측 상단 '작업' 버튼 클릭 → 'VPC에 연결' 버튼 클릭'**
- **사용 가능한 VPC 선택 (lab-edu-vpc-ap-01) → '인터넷 게이트웨이 연결' 버튼 클릭'**

![image](./img/route_03.png)



## Routing Table 설정
### 1. Public & Private Routing Table 생성
- 아래 라우팅 테이블 명세서를 참고하여 정보 입력
|    |Public Routing Table|Private Routing Table 01|Private Routing Table 02|
|:---:|:---:|:---:|:---:|
|Name|lab-edu-rtb-pub|lab-edu-rtb-pri-01|lab-edu-rtb-pri-02|
|VPC |lab-edu-vpc-ap-01|lab-edu-vpc-ap-01|lab-edu-vpc-ap-01|

<img src="./img/table_01.png" width="600" />

### 2. Public Routing Table 설정
- **라우팅 테이블 리소스 탭 → lab-edu-rtb-pub 선택 → '서브넷 연결' 탭 선택 (화면 하단) → '서브넷 연결 편집' 버튼 클릭**

![image](./img/table_02.png)

- **'lab-edu-sub-pub-01', 'lab-edu-sub-pub-02' 선택 → '연결 저장' 버튼 클릭**

![image](./img/table_03.png)

- **'lab-edu-rtb-pub' 선택 → '라우팅' 탭 클릭(화면 하단) → '라우팅 편집' 버튼 클릭**

![image](./img/table_04.png)

- 아래 라우팅 테이블 경로 명세서를 참고하여 정보 입력
    - '라우팅 추가' 버튼 클릭
    - 대상: 0.0.0.0/0
    - 대상: lab-edu-igw-ap (internet gateway)
    - '변경 사항 저장' 버튼 클릭

![image](./img/table_05.png)



## Nat Gateway 생성
### 1. Nat Gateway 리소스 탭으로 이동 → 'Nat Gateway 생성' 버튼 클릭
- 아래 Nat Gateway 명세서를 참고하여 정보 입력
    - 이름: lab-edu-natgw-01
    - 서브넷: lab-edu-sub-pub-01 (Nat Gateway를 설치할 서브넷 위치)
    - '탄력적 IP 할당' 버튼 클릭

<img src="./img/gateway.png" width="600" />



## Private Routing Table 설정
- **라우팅 테이블 리소스 탭 → lab-edu-rtb-pri-01 선택 → '서브넷 연결' 탭 선택 (화면 하단) → '서브넷 연결 편집' 버튼 클릭**

![image](./img/table_06.png)

- **'lab-edu-sub-pri-01' 선택 → '연결 저장' 버튼 클릭**

![image](./img/table_07.png)

**※ 위 과정을 동일하게 한 번 더 진행하면서 lab-edu-<span style="color:orange">rtb</span>-pri-02에 lab-edu-<span style="color:orange">sub</span>-pri-02 연결**

- **'lab-edu-rtb-pri-01' 선택 → '라우팅' 탭 클릭(화면 하단) → '라우팅 편집' 버튼 클릭**

![image](./img/table_08.png)

- 아래 라우팅 테이블 경로 명세서를 참고하여 정보 입력
    - '라우팅 추가' 버튼 클릭
    - 대상: 0.0.0.0/0
    - 대상: lab-edu-natgw-01 (nat gateway)
    - '변경 사항 저장' 버튼 클릭

![image](./img/table_09.png)