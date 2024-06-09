# Creating Database

### 1. Database Subnet 생성

- **VPC 메인 콘솔 화면 → Subnet 리소스 탭 → "Subnet 생성" 버튼 클릭**

- 아래 서브넷 자원 명세서를 참고하여 정보 입력

- 화면 하단의 '서브넷 추가' 버튼 이용 여러 개의 서브넷 정보 동시 입력 가능 

    |                   |Database Subnet 01|Database Subnet 02|
    |:---:|:---:|:---:|
    |VPC_ID             |leb-edu-vpc-ap-01|leb-edu-vpc-ap-01|
    |Subnet_Name        |lab-edu-sub-db-01|lab-edu-sub-db-02|
    |Availability_Zone  |ap-northeast-2a|ap-northeast-2c|
    |IPv4 CIDR          |10.0.80.0/24|10.0.81.0/24|

### 2. Security Group 생성

- **EC2 메인 콘솔 화면 → 보안 그룹 탭 → '보안그룹 생성' 버튼 클릭**

- 아래 **보안그룹 자원 명세서** 정보 참고하여 설정

    |Region         |VPC_Name           |Name               |Rule       |port   |Protocol   |Source|
    |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
    |ap-northeast-2 |lab-edu-vpc-ap-01  |lab-edu-sg-aurora  |In-bound   |5432   |Postgres   |10.0.0.0/16|

### 3. Subnet Group 생성

- **RDS 메인 콘솔 화면 → 서브넷 그룹 탭 → 'DB 서브넷 그룹 생성' 버튼 클릭**

    ![alt text](./img/db_subnet_group_01.png)

- 아래 정보 참고하여 설정

    - 이름: lab-edu-subgroup-aurora

    - VPC: lab-edu-vpc-ap-01

    - 가용영역: ap-northeast-2a, ap-northeast-2c

    - 서브넷: lab-edu-sub-db-01, lab-edu-sub-db-02

        ![alt text](./img/db_subnet_group_02.png)

### 4. RDS 생성

- **RDS 메인 콘솔 화면 → 데이터베이스 탭 → '데이터베이스 생성' 버튼 클릭**

- 아래 **데이터베이스 자원 명세서** 정보 참고하여 설정

    - 엔진유형: Aurora (PostgreSQL Compatible)

    - 사용 가능한 버전: Aurora PostgreSQL 15.3

    - 템플릿: 개발/테스트

    - DB 클러스터 식별자: lab-edu-rds-aurora

    - Master User Name: postgres

    - Master User Password: qwer1234

        ![alt text](./img/db_aurora_01.png)
        
    - 인스턴스 구성: 버스터블 클래스(t클래스 포함) - db.t3.medium

    - 다중 AZ 배포: 다른 AZ에 Aurora 복제본/리더 노드 생성 (확장된 가용성에 권장)

    - VPC: lab-edu-vpc-ap-01

    - DB 서브넷 그룹: lab-edu-subgroup-aurora

        ![alt text](./img/db_aurora_02.png)
        
    - 퍼블릭 액세스: 아니오

    - VPC 보안 그룹: lab-edu-sg-aurora

    - 데이터베이스 인증: 암호 인증

    - 모니터링 Enhanced 모니터링 활성화 체크박스 해제

    - 나머지 항목은 Default 값으로 설정하고 '생성' 버튼 클릭

        ![alt text](./img/db_aurora_03.png)
