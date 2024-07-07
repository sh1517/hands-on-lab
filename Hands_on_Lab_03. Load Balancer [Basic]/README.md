# Load Balancer 생성

### 1. Security Group 생성

- **EC2 메인 콘솔 화면 → 보안 그룹 탭 → '보안그룹 생성' 버튼 클릭**

- 아래 **보안그룹 자원 명세서** 정보 참고하여 생성 정보 입력

    |Region|        VPC_Name|         Name|          Rule|    port| Protocol|   Source|
    |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
    |ap-northeast-2|lab-edu-vpc-ap-01|lab-edu-sg-alb|In-bound|80|   HTTP|       0.0.0.0/0|
    |   |   |                                       |In-bound|443|  HTTPS|       0.0.0.0/0|
    
### 2. Load Balancer Target Group 생성

- **EC2 메인 콘솔 화면 → 대상 그룹 탭 → 'Create Target Group' 버튼 클릭**

- 아래 **타겟 그룹 자원 정보** 참고하여 정보 입력

    - **대상 유형 선택 :** *instances*

    - **대상 그룹 이름:** *lab-edu-tg-web*

    - **VPC:** *lab-edu-vpc-ap-01*

    - '**Next**' 버튼 클릭

        <img src="./img/lb_01.png" width="1000" />

    - **대상 등록 설정**

        - '**lab-edu-ec2-web**' 서버 체크박스 활성화

        - '**아래에 보류 중인 것으로 포함**' 버튼 클릭

            <img src="./img/lb_02.png" width="960" />

    - 나머지 설정은 **Default** 값으로 유지하고 '생성' 
    
### 2. Load Balancer 생성

- **EC2 메인 콘솔 화면 → 로드밸런서 탭 → '로드밸런서 생성' 버튼 클릭 → 'Application Load Balancer' 선택**

- 아래 **로드밸런서 자원 정보** 참고하여 정보 입력

    - **로드밸런서 이름:** *lab-edu-alb-web*

    - **VPC:** *lab-edu-vpc-ap-01*

    - '**ap-northeast-2a**' 체크박스 활성화 → '**lab-edu-sub-pub-01**' 선택

    - '**ap-northeast-2c**' 체크박스 활성화 → '**lab-edu-sub-pub-02**' 선택

        ![alt text](./img/lb_03.png)

    - **보안 그룹:** *lab-edu-sg-alb*

    - **리스너** 

        - **Protocol:** HTTP
        
        - **Port:** 80

        - **Default Action:** 'lab-edu-tg-web'

    - 나머지 설정은 **Default** 값으로 유지하고 '생성' 

        ![alt text](./img/lb_04.png)

    - 웹 서비스 접속 테스트 (로드밸런서 DNS 정보로 브라우저에서 접속)

        ![alt text](./img/web_service_01.png)
        ***※ Web Server 에는 EC2 관련 데이터 접근을 위한 IAM 권한이 없어 에러 페이지 반환***