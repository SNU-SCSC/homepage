## 글 쓰기

### 요청

#### URL

  `POST /boards/{board_id}/articles`

#### URL 파라미터

  * `board_id`: 문자열

#### 데이터 파라미터

  * `subject`: 비지 않은 문자열
  * `content`: 문자열
  * `isDraft`: 참/거짓

#### 헤더
  * `x-access-token`

### 성공 시 응답

#### 응답 코드

201 (Created)

#### 응답 헤더

  * `Location`: `/boards/{board_id}/articles/{article_id}`

#### 응답 인터페이스 예시

```typescript
interface CreateArticleResponseSuccess {
  article_id: number
  board_id: string
  writer: string
  seq: number
  subject: string
  content: string
  hit: number
  isDeleted: boolean
  isDraft: boolean
  created_at: string
  updated_at: string
};
```

#### 응답 예시

```json
{
  "article_id": 5615613,
  "board_id": "y4pZwF",
  "writer": "scsc",
  "seq": 1,
  "subject": "게시글 제목",
  "content": "",
  "hit": 0,
  "isDeleted": false,
  "isDraft": false,
  "created_at": "2022-09-01T12:34:56Z",
  "updated_at": "2022-09-01T12:34:56Z",
}
```

### 실패 시 응답 &ndash; 권한 없음

#### 조건

글을 작성하려는 게시판이 회원만 쓸 수 있는 게시판이고 사용자가 로그인하지 않았을 때,
또는 글을 작성하려는 게시판이 특정 등급의 회원만 쓸 수 있는 게시판이고 사용자가 그 등급에 속하지 않을 때

#### 응답 코드

200 (OK)

#### 응답 인터페이스 예시

```typescript
interface CreateArticleResponseUnauthorized {
  error: {
    id: "ERROR_UNAUTHORIZED"
    message: string
  }
};
```

#### 응답 예시

```json
{
  "error": {
    "id": "ERROR_UNAUTHORIZED",
    "message": "You are unauthorized to make this request."
  }
}
```

### 호출 예시

```typescript
interface CreateArticleRequestBody {
  subject: string
  content: string
  isDraft: boolean
};

type CreateArticleResponse = |
| CreateArticleResponseSuccess
| CreateArticleResponseUnauthorized;

const body: CreateArticleRequestBody = {
  subject: '...',
  content: '...',
  isDraft: true
};

fetch(new Request(
  `/boards/${boardId}/posts`, {
    method: 'POST',
    body: new Blob([JSON.stringify(body)], {type: 'application/json'}),
  }
)).then(
  (response) => response.json() as CreateArticleResponse
).then(
  (data) => {
    if (data instanceof CreateArticleResponseSuccess) {
      console.log(data);
    } else if (data instanceof CreateArticleResponseUnauthorized) {
      console.log(data.error.message)
    }
  }
);
```
