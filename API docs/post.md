# 글

* 게시판 id를 이용하여 접속

## 게시판에서 글 찾기

게시글의 제목과 게시글 id, 조회수 목록을 반환

### 요청

#### URL

  `GET /boards/{board_id}`

#### URL 파라미터

* `board_id`: 문자열

#### 데이터 파라미터

* `page`: 1 이상의 정수, 기본값 1
* `page_size`: 1 이상 100 이하의 정수, 기본값 20

### 성공 시 응답

#### 응답 코드

200 (OK)

#### 응답 인터페이스 예시

```typescript
interface FindPostResponseSuccessEntry {
  post_id: string
  post_name: string
  post_hit: number
};

interface FindPostResponseSuccess {
  documents: FindPostResponseSuccessEntry[]
};
```

#### 응답 예시

```json
{
  "documents": [
    {
      "post_id": "M2XUa3",
      "post_name": "Lorem ipsum",
      "post_hit": 12
    },
    {
      "post_id": "ohcZjX",
      "post_name": "dolor sit amet",
      "post_hit": 11
    },
    {
      "post_id": "aVgUwg",
      "post_name": "consectetur adipiscing elit",
      "post_hit": 13
    },
    {
      "post_id": "WNUa99",
      "post_name": "sed do eiusmod",
      "post_hit": 19
    },
    {
      "post_id": "4mZsxP",
      "post_name": "tempor incididunt",
      "post_hit": 8
    },
    ...
  ]
}
```

### 실패 시 응답 &ndash; 권한 없음

#### 조건

게시판이 회원만 읽을 수 있는 게시판이고 사용자가 로그인하지 않았을 때,
또는 게시판이 특정 등급의 회원만 읽을 수 있는 게시판이고 사용자가 그 등급에 속하지 않을 때

#### 응답 코드

200 (OK)

#### 응답 인터페이스 예시

```typescript
interface FindPostResponseUnauthorized {
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
FindPostResponse = |
| FindPostResponseSuccess
| FindPostResponseUnauthorized;

fetch(new Request(
  `/boards/${boardId}?${new URLSearchParams({page: page, page_size: pageSize})}`,
)).then(
  (response) => response.json() as FindPostResponse
).then(
  (data) => {
    if (data instanceof FindPostResponseSuccess) {
      console.log(data.documents)
    } else if (data instanceof FindPostResponseUnauthorized) {
      console.log(data.error.message)
    }
  }
);
```

## 글 보기

### 요청

#### URL

  `GET /boards/{board_id}/{post_id}`

#### URL 파라미터

* `board_id`: 문자열
* `post_id`: 문자열
