# Lambda 생성 및 기본 사용방법 

### 1. Lambda 함수 생성 및 코드 수정

- **Lambda 메인 콘솔 화면 → "함수 생성" 버튼 클릭**

- 함수 생성 정보 입력

    - 함수 이름: lab-edu-lambda-event-handler

    - 런타임: python 3.10

    - '함수 생성' 버튼 클릭

        ![alt text](./img/lambda_01.png)

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

- 'serverless_code/lambda' 폴더의 'lab-edu-lambda-event-handler.py' 파일 열기 → 코드 복사

    ![alt text](./img/lambda_02.png)

- **Lambda 메인 콘솔 화면 → "lab-edu-lambda-event-handler" 선택 → 코드 소스 항목에 붙여넣기 → 'Deploy' 버튼 클릭**

    ![alt text](./img/lambda_03.png)

### 2. Test Event 구성 및 테스트 

- 'Test' 버튼 클릭

    ![alt text](./img/lambda_04.png)

- 이벤트 생성 정보 입력

    - 이벤트 이름: lab-edu-event-hello

    - 이벤트 JSON:

        ```json
        {
            "key1": "hello-lambda",
            "key2": "lambda python platform",
            "key3": "event test code"
        }
        ```

    - '저장' 버튼 클릭

        ![alt text](./img/lambda_05.png)

- 'Test' 버튼 클릭 → 출력 결과 확인

    ![alt text](./img/lambda_06.png)



