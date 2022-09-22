## 댓글 쓰기
### 요청
#### URL
 `POST /boards/{board_id}/{article_id}/comments`

#### 헤더
  * `x-access-token`

#### data 파라미터
  **Required:**
  ` content=[string]`

 **Not Required:**
  `parent=[integer]`
<br>
<br>
#### 응답

**Success ResponseCode:** `201(created)`

  **Content:**
   `{id:2, content="abc",parent=1}`

<br>

**Error Response**
  **Code:** `404(Not Found)`

  **Content:**`{error:'no such article or parent comment'}`
   <br>
   <br>

  **Code:** `200(UnAuthorized)`

  **Content:**`{error:'권한이 없는 사용자입니다.}`

<br>

  **Code:** `400(Bad Request)`

  **Content:**:`{error:'필수 사항이 부족합니다'}`

## 댓글 삭제하기
### 요청
#### URL
`DELETE /comment/{id}`
#### URL 파라미터
 Required:id[integer]

 **Success Response**
    **Code**: `204(No content)`
    <br>
    
  **Error response**
  **code**: `401(Unauthorized)`
  **Content:**`{error:'권한이 없는 사용자입니다.}`
  <br>

  **code**: `404(Not found)`
  **content:**`{error:'not found}`



