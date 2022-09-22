# 게시판 생성 api

  게시판 생성 전 권한 확인을 위해 권한을 확인하고, 생성될 게시판에 대한 정보를 저장합니다.
  
  + 카카오스토리 api문서를 참고하였습니다. 안드로이드용 api라 pc로 변형해야될 부분이 남아있을 수 있어 이에 대한 수정이 필요할 듯 합니다. 또한 게시판을 관리자가 추가하기 쉽도록 가이드라인만드는 것이 필요해 보입니다.
  

 ## 사용자 계정 조사
 
 접속한 사용자의 계정이 게시판을 만들 수 있는 권한을 가지고 있는지 확인합니다.
 

### URL

    GET ~/user/userType HTTP/1.1
    Host:
    Authorization:
    Content-type:
    
### URL 파라미터

    None

### Data 파라미터

   Required:
     
   `userType=[string]`

### Success Response:

   Code: 
   
    200 (OK)
    
    "isQualifiedUser": True
 
### Error Response:

  Code: 
  
    404 NOT FOUND 
    
      { error : "Page doesn't exist" }
      
#### OR

   Code: 
   
    401.1 UNAUTHORIZED
    
    { error: "권한이 없는 사용자 입니다." }

### Sample:

 Request
  ```javascript
        curl -v -X GET "~~/user/userType" \
            -H "Authorization: Bearer ${ACCESS_TOKEN}"
          
  ```
  
 Response
  ```javascript
         HTTP/1.1 200 OK
         Content-Type: application/json;charset=UTF-8
         {
            "isQualifiedUser": True
         }
  ```

 ## 게시판 작성 내용 전송
 
 사용자가 입력한 내용을 전달합니다.
 
### URL

    POST  ~/borad/bordmake HTTP/1.1
    Host:
    Authorization:
    Content-type:

### Data 파라미터

   Required:
   
    boardName=[string],
    higherBoard=[string],
    readQulify=[string],
    writeQulify=[string],
    commentQulify=[string]
   
   not Required:
    
    boardIntroducion=[string]

### Success Response:

  #### Code: 
      200 (OK)
          
          id: "[생성된게시판_String_id]"
 
### Error Response:

  #### Code:
      404 NOT FOUND
      
        error : "Page doesn't exist"

  #### OR

 
  #### Code: 
      406 Not Acceptable
      
        error: "필수 사항이 부족합니다"

### Sample:

Request
  ```javascript
        curl -v -X POST "~/board/boradmake" \
            -H "Content-Type: \
            -H "Authorization: Bearer ${ACCESS_TOKEN}" \
            --data-urlencode "[게시판이름]" \
            --data-urlencode "[상위게시판]" \
            --data-urlencode "[읽기권한]" \
            --data-urlencode "[쓰기권한]" \
            --data-urlencode "[댓글권한]" \
            --data-urlencode "[게시판소개]" 
  ```
  
Response
  ```javascript
        HTTP/1.1 200 OK
        Content-Type: application/json;charset=UTF-8
        {
            "id":"[만들어질게시판_id]"
         }
  ```

## 게시판 작성 내용 임시 저장
 
 사용자가 입력한 내용을 저장합니다.
 
 + 게시판 사용자가 만든 내용을 서버에 저장한다면 어디 위치에 저장할지 정해야합니다. 또한 '게시판 만들기' 페이지에서 임시 게시판 불러오기 기능을 추가해야할듯 합니다. 임시로 불러오기 api로 만들어 봤습니다.

### URL

    POST  ~/borad/boardsave HTTP/1.1
    Host:
    Authorization: Bearer ${ACCESS_TOKEN}
    Content-type: [필요시작성]

### Data 파라미터

   not Required:
   
    boardName=[string],
    higherBoard=[string],
    readQulify=[string],
    writeQulify=[string],
    commentQulify=[string],    
    boardIntroducion=[string]

### Success Response:

  #### Code: 
      200 (OK)
          
          id: "[생성된게시판_String_id]"
 
### Error Response:

  #### Code:
      404 NOT FOUND
      
        error : "Page doesn't exist"

### Sample:

Request
  ```javascript
        curl -v -X POST "~/board/boradsave" \
            -H "Content-Type: \
            -H "Authorization: Bearer ${ACCESS_TOKEN}" \
            --data-urlencode "boardName=SCSC중고장터" \
            --data-urlencode "higherBoard=홍보/모집" \
            --data-urlencode "readQulify=준회원" \
            --data-urlencode "writeQulify=준회원" \
            --data-urlencode "commentQulify=준회원" \
            --data-urlencode "boardIntroducion=Sell&Buy everyting You want!" 
  ```
  
Response
  ```javascript
        HTTP/1.1 200 OK
        Content-Type: application/json;charset=UTF-8
        {
            "id": "[만들어질게시판_id]"
        }
  ```

## 게시판 임시 작성 내용 불러오기

  사용자가 입력한 게시판 내용을 서버에서 불러옵니다.

### URL

    GET  ~/borad/boardload HTTP/1.1
    Host:
    Authorization: Bearer ${ACCESS_TOKEN}

### Data 파라미터

    id: "[생성된게시판_String_id]"


### Success Response:

  #### Code: 
      200 (OK)
   
        boardName=[string],
        higherBoard=[string],
        readQulify=[string],
        writeQulify=[string],
        commentQulify=[string],
        boardIntroducion=[string]
 
### Error Response:

  #### Code:
      404 NOT FOUND
      
        error : "Page doesn't exist"

### Sample:

Request
  ```javascript
        curl -v -G GET "~/board/boardload" \
            -H "Authorization: Bearer ${ACCESS_TOKEN}" \
            -d "id=${BOARD_ID}"
  ```
  
Response
  ```javascript
        HTTP/1.1 200 OK
        Content-Type: application/json;charset=UTF-8
        {
          "id":"AAAA.BBBB"
          "boardName":"SCSC중고장터" \
          "higherBoard":"홍보/모집" \
          "readQulify":"준회원" \
          "writeQulify":"준회원" \
          "commentQulify":"준회원" \
          "boardIntroducion":"Sell&Buy everyting You want!" 
         }
  ```