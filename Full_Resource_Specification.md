- **네트워크 자원 명세서** 

    |Region|VPC_Name|CIDR|Subnet_Name|CIDR|Routing_Table_Name|
    |:---:|:---:|:---:|:---:|:---:|:---:|
    |ap-northeast-2|lab-edu-vpc-ap-01|10.0.0.0/16 |lab-edu-sub-pub-01|10.0.0.0/24 |lab-edu-rtb-pub    |
    |   |   |                                     |lab-edu-sub-pub-02|10.0.1.0/24 |lab-edu-rtb-pub    |
    |   |   |                                     |lab-edu-sub-pri-01|10.0.40.0/24|lab-edu-rtb-pri-01 |
    |   |   |                                     |lab-edu-sub-pri-02|10.0.41.0/24|lab-edu-rtb-pri-02 |
    |   |   |                                     |lab-edu-sub-db-01 |10.0.80.0/24|lab-edu-rtb-db|
    |   |   |                                     |lab-edu-sub-db-02 |10.0.81.0/24|lab-edu-rtb-db|
    |   |   |                                     |lab-edu-sub-tgw-01|10.0.255.224/24|lab-edu-rtb-tgw|
    |   |   |                                     |lab-edu-sub-tgw-02|10.0.255.240/24|lab-edu-rtb-tgw|
    |ap-northeast-2|lab-edu-vpc-ap-02|10.10.0.0/16|lab-edu-sub-pub-01|10.10.0.0/24 |lab-edu-rtb-pub |
    |   |   |                                     |lab-edu-sub-pub-02|10.10.1.0/24 |lab-edu-rtb-pub |
    |   |   |                                     |lab-edu-sub-pri-01|10.10.40.0/24|lab-edu-rtb-pri |
    |   |   |                                     |lab-edu-sub-pri-02|10.10.41.0/24|lab-edu-rtb-pri |
    |   |   |                                     |lab-edu-sub-db-01|10.10.80.0/24|lab-edu-rtb-db|
    |   |   |                                     |lab-edu-sub-db-02|10.10.81.0/24|lab-edu-rtb-db|
    |   |   |                                     |lab-edu-sub-tgw-01|10.10.255.224/24|lab-edu-rtb-tgw|
    |   |   |                                     |lab-edu-sub-tgw-02|10.10.255.240/24|lab-edu-rtb-tgw|
    |us-east-1|lab-edu-vpc-us-01|10.20.0.0/16     |lab-edu-sub-pub-01|10.10.0.0/24 |lab-edu-rtb-pub |
    |   |   |                                     |lab-edu-sub-pub-02|10.10.1.0/24 |lab-edu-rtb-pub |
    |   |   |                                     |lab-edu-sub-pri-01|10.10.40.0/24|lab-edu-rtb-pri |
    |   |   |                                     |lab-edu-sub-pri-02|10.10.41.0/24|lab-edu-rtb-pri |
    |   |   |                                     |lab-edu-sub-db-01|10.10.80.0/24|lab-edu-rtb-db|
    |   |   |                                     |lab-edu-sub-db-02|10.10.81.0/24|lab-edu-rtb-db|
    |   |   |                                     |lab-edu-sub-tgw-01|10.10.255.224/24|lab-edu-rtb-tgw|
    |   |   |                                     |lab-edu-sub-tgw-02|10.10.255.240/24|lab-edu-rtb-tgw|
    |eu-central-1|lab-edu-vpc-eu-01|10.30.0.0/16  |lab-edu-sub-pub-01|10.10.0.0/24 |lab-edu-rtb-pub |
    |   |   |                                     |lab-edu-sub-pub-02|10.10.1.0/24 |lab-edu-rtb-pub |
    |   |   |                                     |lab-edu-sub-pri-01|10.10.40.0/24|lab-edu-rtb-pri |
    |   |   |                                     |lab-edu-sub-pri-02|10.10.41.0/24|lab-edu-rtb-pri |
    |   |   |                                     |lab-edu-sub-db-01|10.10.80.0/24|lab-edu-rtb-db|
    |   |   |                                     |lab-edu-sub-db-02|10.10.81.0/24|lab-edu-rtb-db|
    |   |   |                                     |lab-edu-sub-tgw-01|10.10.255.224/24|lab-edu-rtb-tgw|
    |   |   |                                     |lab-edu-sub-tgw-02|10.10.255.240/24|lab-edu-rtb-tgw|


- **보안그룹 자원 명세서**

    |Region|        VPC_Name|         Name|          Rule|        port|Protocol|Source|
    |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
    |ap-northeast-2|lab-edu-vpc-ap-01|lab-edu-sg-bastion|In-bound|22|   SSH|    MY_PUBLIC_IP|
    |   |   |                                           |In-bound|80|   HTTP|   MY_PUBLIC_IP|
    |ap-northeast-2|lab-edu-vpc-ap-01|lab-edu-sg-web    |In-bound|22|   SSH|    10.0.0.0/16|
    |   |   |                                           |In-bound|80|   HTTP|   10.0.0.0/16|
    |ap-northeast-2|lab-edu-vpc-ap-01|lab-edu-sg-alb    |In-bound|80|   HTTP|   0.0.0.0/0|
    |   |   |                                           |In-bound|443|  HTTPS|  0.0.0.0/0|