# Serverless 웹 서비스 구성

### 1. Dynamo DB 생성

- **Dynamo DB 콘솔 메인 화면 → "테이블 생성" 버튼 클릭**

- 테이블 생성 정보 입력

    - 테이블 이름: lab-edu-dynamodb-todo-list
    
    - 파티션 키: todo

    - '테이블 생성' 버튼 클릭

        ![alt text](./img/dynamo_01.png)

### 2. Lambda 함수 생성 및 코드 수정

- **Lambda 메인 콘솔 화면 → "함수 생성" 버튼 클릭**

- Data Insert 함수 생성 정보 입력

    - 함수 이름: lab-edu-lambda-serverless-put

    - 런타임: python 3.10

    - '기본 실행 역할 변경' 확장

        ![alt text](./img/lambda_01.png)

    - 실행 역할 생성 정보 입력

        - 'AWS 정책 템플릿에서 새 역할 생성' 라디오 버튼 클릭

        - 역할 이름: lab-edu-role-lambda

        - 정책 템플릿: '단순 마이크로 서비스 권한'

            ![alt text](./img/lambda_02.png)

    - '함수 생성' 버튼 클릭

- Cloud9 IDE Terminal 화면으로 이동 → 폴더 구조 확인

    ```bash
    cloud-wave-workspace/
    ├── images
    ├── scripts
    ├── serverless_code
    │   ├── css
    │   │   └── style.css
    │   ├── images
    │   │   └── cj-olivenetworks.png
    │   ├── index.html
    │   ├── lambda
    │   │   ├── lab-edu-lambda-event-handler.py
    │   │   ├── lab-edu-lambda-serverless_delete.py
    │   │   └── lab-edu-lambda-serverless_put.py
    │   └── src
    │       └── script.js
    └── support_files
    ```

- 'serverless_code/lambda' 폴더의 'lab-edu-lambda-serverless_put.py' 파일 열기 → 코드 복사

    ```python
    import json
    import boto3
    from botocore.exceptions import ClientError

    # DynamoDB 리소스 초기화
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('lab-edu-dynamodb-todo-list')  # DynamoDB 테이블 이름으로 대체

    def lambda_handler(event, context):
        try:
            todo = event['todo']
        except (KeyError, TypeError, json.JSONDecodeError):
            return {
                'statusCode': 400,
                'body': json.dumps('Invalid request')
            }
        table.put_item(Item={'todo': todo})    
        
        try:
            response = table.scan()
            items = response['Items']
            print(items)
            data = {
                'statusCode': 200,
                'headers': {'Access-Control-Allow-Origin': '*'},
                'body': json.dumps(items)
            }
            return data
        except Exception as e:
            return {
                'statusCode': 500,
                'body': json.dumps('Error retrieving data from DynamoDB: ' + str(e))
            }
    ```

- **Lambda 메인 콘솔 화면 → "lab-edu-lambda-serverless-put" 선택 → 코드 소스 항목에 붙여넣기 → 'Deploy' 버튼 클릭**

    ![alt text](./img/lambda_03.png)

- **Lambda 메인 콘솔 화면 → "함수 생성" 버튼 클릭**

- Data Delete 함수 생성 정보 입력

    - 함수 이름: lab-edu-lambda-serverless-delete

    - 런타임: python 3.10

    - '기본 실행 역할 변경' 확장

    - 실행 역할 생성 정보 입력

        - '기존 역할 사용' 라디오 버튼 클릭

        - 역할 이름: lab-edu-role-lambda

    - '함수 생성' 버튼 클릭

- Cloud9 IDE Terminal 화면으로 이동 → 폴더 구조 확인

    ```bash
    cloud-wave-workspace/
    ├── images
    ├── scripts
    ├── serverless_code
    │   ├── css
    │   │   └── style.css
    │   ├── images
    │   │   └── cj-olivenetworks.png
    │   ├── index.html
    │   ├── lambda
    │   │   ├── lab-edu-lambda-event-handler.py
    │   │   ├── lab-edu-lambda-serverless_delete.py
    │   │   └── lab-edu-lambda-serverless_put.py
    │   └── src
    │       └── script.js
    └── support_files
    ```

- 'serverless_code/lambda' 폴더의 'lab-edu-lambda-serverless_delete.py' 파일 열기 → 코드 복사

    ```python
    import json
    import boto3

    # DynamoDB 초기화
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('lab-edu-dynamodb-todo-list')

    def lambda_handler(event, context):
        try:
            # 요청 본문에서 삭제할 항목의 키 추출
            todo = event['todo']

            # DynamoDB에서 항목 삭제
            table.delete_item(
                Key={'todo': todo}
            )

            return {
                'statusCode': 200,
                'body': json.dumps('Item deleted successfully')
            }
        except Exception as e:
            return {
                'statusCode': 500,
                'body': json.dumps('Error deleting item')
            }
    ```

- **Lambda 메인 콘솔 화면 → "lab-edu-lambda-serverless-delete" 선택 → 코드 소스 항목에 붙여넣기 → 'Deploy' 버튼 클릭**

### 3. API Gateway 생성

- **API Gateway 메인 콘솔 화면 → REST API 영역의 "구축" 버튼 클릭**

    ![alt text](./img/api_01.png)

- REST API 생성 정보 입력

    - '새 API' 라디오 버튼 클릭

    - API 이름: lab-edu-apigw-serverless

    - 'API 생성' 버튼 클릭

        ![alt text](./img/api_02.png)

- '리소스 생성' 버튼 클릭

    ![alt text](./img/api_03.png)

- 리소스 생성 정보 입력

    - 리소스 이름: insert

    - '오리진 간 리소스 공유(CORS)' 체크박스 클릭

    - '리소스 생성' 버튼 클릭

        ![alt text](./img/api_04.png)

- '메서드 생성' 버튼 클릭

    ![alt text](./img/api_05.png)

- 메서드 생성 정보 입력

    - 메서드 유형: POST

    - 통합 유형: Lambda

    - Lamdba 함수: lab-edu-lambda-serverless-put

        ![alt text](./img/api_06.png)

- 리소스의 '/insert' 항목 클릭 → 'CORS' 활성화 버튼 클릭

    ![alt text](./img/api_07.png)

- Access-Control-Allow-Methods 수정
 
    - OPTION 활성화
    
    - POST 활성화 

    - '저장' 버튼 클릭

        ![alt text](./img/api_08.png)

- 'API 배포' 버튼 클릭

    - 스테이지: new

    - 스테이지 이름: dev

        ![alt text](./img/api_09.png)

- API 'URL 호출' 정보 복사

    ![alt text](./img/api_10.png)

- Cloud9 IDE 화면으로 이동 → 폴더 구조 확인


    ```bash
    cloud-wave-workspace/
    ├── images
    ├── scripts
    ├── serverless_code
    │   ├── css
    │   │   └── style.css
    │   ├── images
    │   │   └── cj-olivenetworks.png
    │   ├── index.html
    │   ├── lambda
    │   │   ├── lab-edu-lambda-event-handler.py
    │   │   ├── lab-edu-lambda-serverless_delete.py
    │   │   └── lab-edu-lambda-serverless_put.py
    │   └── src
    │       └── script.js
    └── support_files
    ```

- 'serverless_code/lambda' 폴더의 'script.js' 파일 열기 → 코드 수정

    ```javascript
    document.getElementById('addBtn').addEventListener('click', addTodo)

    function addTodo() {
        const todoInput = document.getElementById('todoInput').value;

        if(todoInput) {
            fetch('{API_GATEWAAY_INSERT_URL}', {    // API 'URL 호출' 정보 붙여넣기
                method: 'POST',
                body: JSON.stringify({ todo: todoInput }),
                headers: { 'Content-Type': 'application/json' }
            })
            .then(response => response.json())
            .then(data => {
                addListItem(data.body);
            })
            .catch((error) => {
                console.error('Error:', error);
            });
        };
    };
    ```

- Cloud9 IDE Terminal 에서 S3로 변경된 소스 코드 업로드

    ```bash
    cd ~/environment/serverless_code
    aws s3 sync ./ s3://s3.{st01~30}.cj-cloud-wave.com
    ```

### 4. 웹 호스팅 접속 테스트 (http://s3.{st01~30}.cj-cloud-wave.com/ 접속)

- http://s3.{st01~30}.cj-cloud-wave.com/ 접속 → 데이터 입력 → 'Add' 버튼 클릭

![alt text](./img/test_01.png)

- 동작하지 않는 경우 CloudFront 캐시 무효화 처리 진행

- CloudFront 콘솔 메인 화면 → 대체 도메인: 's3.cj-cloud-wave.com' 클릭 → '무효화' 탭 → '무효화 생성' 버튼 클릭

    ![alt text](./img/test_02.png)

- '.*' 입력 → '무효화 생성' 버튼 클릭


