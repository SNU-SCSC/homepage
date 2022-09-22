# 로그인/회원가입 REST API
- JWT(JSON Web Token) 인증방식을 사용.
- auth API와 user API로 구분.
___

## 1. Auth - login
- **Endpoint**: [POST] api/auth/login
- **Description**: username과 password로 API에 로그인, token을 return.
- **Request Example**:  
URL: [POST] api/auth/login  
Body:
    ```ts
    {
        "username": "test1",
        "password": "Password1"
    }
    ```
- **Response Example**: 
    ```ts
    {
        "success": true,
        "message": null,
        "errors": null,
        "data": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1OThkZGI2MzIyYWMxMDExZTA3MDJjYjAiLCJ1c2VybmFtZSI6InRlc3QxIiwibmFtZSI6InRlc3QxIiwiZW1haWwiOiIiLCJpYXQiOjE1MDQ3MzI2NzcsImV4cCI6MTUwNDgxOTA3N30.4eG2zGpSeY2XezKB4Djf6usy7DdygIybR1VKUBj-ScE"
    }
    ```

## 2. Auth - me
- **Endpoint**: [GET] api/auth/me
- **Description**: token을 받아 token의 주인인 user를 return, header에 x-access-token이 요구됨.
- **Request Example**:  
URL: [GET] api/auth/me  
Header:  
    ```ts
    {
        x-access-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1OThkZGI2MzIyYWMxMDExZTA3MDJjYjAiLCJ1c2VybmFtZSI6InRlc3QxIiwibmFtZSI6InRlc3QxIiwiZW1haWwiOiIiLCJpYXQiOjE1MDQ3MzI2NzcsImV4cCI6MTUwNDgxOTA3N30.4eG2zGpSeY2XezKB4Djf6usy7DdygIybR1VKUBj-ScE
    }
    ```  
    Body: *N/A*

- **Response Example**: 
    ```ts
    {
        "success": true,
        "message": null,
        "errors": null,
        "data": {
            "_id": "598ddb6322ac1011e0702cb0",
            "username": "test1",
            "name": "test1",
            "email": "",
            "phoneNumber": "",
            "admissionYear": "", // 입학년도
            "major": "", // 학과
            "memberType": "", // 회원종류 ex)정회원, 준회원, 명예회원 등
            "iat": 1504732677, //토큰이 발급된 시간
            "exp": 1504819077 // 토큰이 만료된 시간
        }
    }
    ```


## 3. Auth - refresh
- **Endpoint**: [GET] api/auth/refresh
- **Description**: 기존의 token을 이용하여 새로운 token을 발급, header에 x-access-token이 요구됨.
- **Request Example**:  
    URL: [GET] api/auth/refresh  
    Header:
    ```ts
    {
        x-access-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1OThkZGI2MzIyYWMxMDExZTA3MDJjYjAiLCJ1c2VybmFtZSI6InRlc3QxIiwibmFtZSI6InRlc3QxIiwiZW1haWwiOiIiLCJpYXQiOjE1MDQ3MzI2NzcsImV4cCI6MTUwNDgxOTA3N30.4eG2zGpSeY2XezKB4Djf6usy7DdygIybR1VKUBj-ScE
    }
    ```
    Body: *N/A*
- **Response Example**:
    ```ts
    {
        "success": true,
        "message": null,
        "errors": null,
        "data": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1OThkZGI2MzIyYWMxMDExZTA3MDJjYjAiLCJ1c2VybmFtZSI6InRlc3QxIiwibmFtZSI6InRlc3QxIiwiZW1haWwiOiIiLCJpYXQiOjE1MDQ3MzQxNzUsImV4cCI6MTUwNDgyMDU3NX0.heYRtT1RZJYqgcJDaWwpKEmFUGwLn2r8OyYX-03_Nx4"
    }
    ```
___

## 4. User - Index
- **Endpoint**: [GET] api/users
- **Description**: user들의 목록을 리턴, header에 x-access-token이 요구됨.  
- **Request Example**:  
    URL: [GET] api/users  
    Header:
    ```ts
    {
        x-access-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1OThkZGI2MzIyYWMxMDExZTA3MDJjYjAiLCJ1c2VybmFtZSI6InRlc3QxIiwibmFtZSI6InRlc3QxIiwiZW1haWwiOiIiLCJpYXQiOjE1MDQ3MzI2NzcsImV4cCI6MTUwNDgxOTA3N30.4eG2zGpSeY2XezKB4Djf6usy7DdygIybR1VKUBj-ScE
    }
    ```
    Body: *N/A*
- **Response Example**:
    ```ts
    {
        "success": true,
        "message": null,
        "errors": null,
        "data": [
            {
                "_id": "598ddb6322ac1011e0702cb0",
                "username": "test1",
                "name": "test1",
                "__v": 0,
                "email": "",
                "phoneNumber": "",
                "admissionYear": "",
                "major": "", 
                "memberType": ""
            },
            {
                "_id": "59a748199e6e4e138033c687",
                "username": "test2",
                "name": "test2",
                "email": "",
                "__v": 0,
                "phoneNumber": "",
                "admissionYear": "",
                "major": "", 
                "memberType": ""
            }
        ]
    }
    ```

## 5. User - Show
- **Endpoint**: [GET] api/users/{username}
- **Description**: username이 {username}인 user를 리턴, header에 x-access-token이 요구됨.
- **Request Example**:  
    URL: [GET] api/users/test1  
    Header:
    ```ts
    {
        x-access-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1OThkZGI2MzIyYWMxMDExZTA3MDJjYjAiLCJ1c2VybmFtZSI6InRlc3QxIiwibmFtZSI6InRlc3QxIiwiZW1haWwiOiIiLCJpYXQiOjE1MDQ3MzI2NzcsImV4cCI6MTUwNDgxOTA3N30.4eG2zGpSeY2XezKB4Djf6usy7DdygIybR1VKUBj-ScE
    }
    ```
    Body: *N/A*
- **Response Example**:
    ```ts
    {
        "success": true,
        "message": null,
        "errors": null,
        "data": {
            "_id": "598ddb6322ac1011e0702cb0",
            "username": "test1",
            "name": "test1",
            "__v": 0,
            "email": "",                
            "phoneNumber": "",
            "admissionYear": "",
            "major": "", 
            "memberType": ""
        }
    }
    ```

## 6. User - Create
- **Endpoint**: [POST] api/users/
- **Description**: body에 username, password, passwordConfirmation, name, email, phoneNumber, major를 받아 새로운 user를 생성.
- **Request Example**:  
    URL: [POST] api/users  
    Body: 
    ```ts
    {
        "username": "test3",
        "password": "password1",
        "passwordConfirmation": "password1",
        "name": "test3",
        "email": "",
        "phoneNumber": "",
        "admissionYear": "",
        "major": "", 
        "memberType": ""
    }
    ```
- **Response Example**:
    ```ts
    {
        "success": true,
        "message": null,
        "errors": null,
        "data": {
            "__v": 0,
            "username": "test3",
            "password": "$2a$10$.UAMVa/QcC.8ckk8sDEZXu1KFhNjINNDJZPx4o9tmaR1kmTmst3Be",
            "name": "test3",
            "email": "",
            "phoneNumber": "",
            "admissionYear": "",
            "major": "", 
            "memberType": "",
            "_id": "59b06d017479637420168a4c"
        }
    }
    ```

## 7. User - Update
- **Endpoint**: [PUT] api/users/{username}
- **Description**: body에 currentPassword, username, name,  
 *(optional) email, phoneNumber, major, newPassword, passwordConfirmation*를 받아 username이 {username}인 user를 수정, header에 x-access-token이 요구됨.
- **Request Example**:  
    URL: [PUT] api/users/test3  
    Header:
    ```ts
    {
        x-access-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1OThkZGI2MzIyYWMxMDExZTA3MDJjYjAiLCJ1c2VybmFtZSI6InRlc3QxIiwibmFtZSI6InRlc3QxIiwiZW1haWwiOiIiLCJpYXQiOjE1MDQ3MzI2NzcsImV4cCI6MTUwNDgxOTA3N30.4eG2zGpSeY2XezKB4Djf6usy7DdygIybR1VKUBj-ScE

    }
    ```
    Body:
    ```ts
    {
        "currentPassword": "Password1",
        "username": "test3",
        "newPassword": "Password1",
        "passwordConfirmation": "Password1",
        "name": "test5",
        "email": "test5@test.com",
        "phoneNumber": "",
        "major": ""
    }
    ```
- **Response Example**:
    ```ts
    {
        "success": true,
        "message": null,
        "errors": null,
        "data": {
            "email": "test5@test.com",
            "name": "test5",
            "username": "test5",
            "_id": "59b06d017479637420168a4c",
            "password": "$2a$10$oEWKzAadbQsxRCNWV4t2lutx8X2m9XtflMCHkD2jYbZEiiGJzVtsG",
            "phoneNumber": "",
            "admissionYear": "",
            "major": "", 
            "memberType": ""
        }
    }
    ```

## 8. User - Destroy
- **Endpoint**: [DELETE] api/users/{username}
- **Description**: username이 {username}인 user를 삭제, header에 x-access-token이 요구됨.
- **Request Example**:
    URL: [PUT] api/users/test3  
    Header:
    ```ts
    {
        x-access-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1OThkZGI2MzIyYWMxMDExZTA3MDJjYjAiLCJ1c2VybmFtZSI6InRlc3QxIiwibmFtZSI6InRlc3QxIiwiZW1haWwiOiIiLCJpYXQiOjE1MDQ3MzI2NzcsImV4cCI6MTUwNDgxOTA3N30.4eG2zGpSeY2XezKB4Djf6usy7DdygIybR1VKUBj-ScE
    }
    ```
    Body: *N/A*
- **Response Example**:
    ```ts
    {
        "success": true,
        "message": null,
        "errors": null,
        "data": {
            "_id": "59b06d017479637420168a4c",
            "username": "test5",
            "name": "test5",
            "email": "test5@test.com",
            "phoneNumber": "",
            "admissionYear": "",
            "major": "", 
            "memberType": "",
            "__v": 0
        }
    }
    ```


