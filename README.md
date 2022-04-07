# 📚 사전 과제: "Toodos"

프론트엔드 경력 채용 면접에 앞서 `사전 과제`가 있습니다.
인터뷰에서 수정 및 구현 작업해주신 내용에 대해 질문을 드릴 예정입니다. 면접에서 편하게 답변을 해주시면 됩니다.

> **중요**: _설명하시는 코드를 함께 볼 수 있도록 google meet에서 **화면 공유 준비**를 부탁드립니다._

## 🎯 목표

본 과제는 개발자의 하루 일과 중 가장 기본적인 업무인 **코드 리뷰** 및 **기능 구현**입니다. 해당 리포지토리는 *Toodos*라는 이름을 가진 `React.js`로 제작된 `To-do 리스트` 앱입니다. 코드 리뷰를 통해 다른 개발자들과 협업하는 스타일과 기존 코드를 리팩토링하는 방식을 파악하고자 합니다. 또한 기능 명세서와 디자인 가이드를 통해 새로운 기능을 어떻게 구현하시는지 파악하기 위해 준비했습니다.

## 🏠 Toodos 구조

`Toodos` 앱의 폴더 구조입니다. `*`(별표)가 있는 파일은 확인 하지 않으셔도 무관합니다.

```javascript
src
 ┣ api
 ┃  ┗ index.js
 ┃  ┗ todo.js
 ┣ components
 ┃  ┣ Header.js
 ┃  ┣ InputTodo.js
 ┃  ┣ TodoItem.js
 ┃  ┗ TodoList.js
 ┣ hooks
 ┃  ┗ useFocus.js
 ┣ pages
 ┃  ┗ Main.js
 ┣ App.css
 ┣ App.js
 ┗ index.js
.env // <--- YOU NEED this!

```

## 👀 코드 리뷰

1. 작성된 코드의 작동 방법을 익히신 후, 개선이 필요하다고 판단되는 부분이 있다면 수정해주세요.
2. 더 나은 프로젝트 구조나, 패턴, 에러 처리, 스타일링 방법 등 자유롭게 작업해주세요.
3. 아래 [검색 Dropdown 구현](#검색-Dropdown-구현) 을 작업해주세요.

> 작업하신 내용은 `GitHub PR` ([Pull Request](https://docs.github.com/es/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request))를 통해 수정한 이유과 내용 등을 정리하여 제출 부탁드립니다.

<br/>

---

<br/>

# 검색 Dropdown 구현

## 🛠 기능 구현

> Dropdown의 아이템은 아래 [API](##API) 를 활용하시면 됩니다.

사용자가 input에 타이핑을 하면 일치하는 아이템들이 dropdown에 보여질 수 있도록 `InputTodo`에 추천 기능을 구현해주시면 됩니다.

1. [디자인 가이드](some_figma_link)를 참고해서 InputTodo의 디자인 수정 및 dropdown을 새로 만들어주세요. (Bootstrap이나 Ant Design, tailwindcss와 같은 UI kit는 사용하지 않고 구현해 주세요.)
2. Input에 `500ms`로 debounce를 적용해주세요.
3. Dropdown에 추천된 아이템들이 처음에 10개가 나올 수 있도록 하고, 아이템이 더 있으면 무한 스크롤로 받아올 수 있도록 구현해주세요.
4. Dropdown에서 아이템 하나를 선택하면, input의 value는 초기화가 되고 아이템이 리스트에 추가되도록 구현해주세요.

<br/>

## 🔍 API

별도로 전달 받으신 api token은 `.env` 파일에 추가 부탁드립니다.

### HTTP

- API: `https://interview-api.labnote.co/api`
- RESOURCE: `{ GET } /todos/search`

### Parameters

| Name  | Required | Type     | Default | Description             |
| ----- | -------- | -------- | ------- | ----------------------- |
| q     | yes      | `string` | -       | input에서 검색하는 단어 |
| page  | no       | `number` | `0`     | 현재 페이지 지정        |
| limit | no       | `number` | `10`    | 받아올 최대 사이즈 값   |

### Responses

| Status | Messsage              | data                                                 |
| ------ | --------------------- | ---------------------------------------------------- |
| 200    | Ok                    | 응답 데이터 (See [Payload result](##Payload-result)) |
| 400    | Bad Request           | `details`: 상세 validation 에러 메시지               |
| 401    | You are unauthorized. | `(인증 실패, 토큰 필요)`                             |
| 500    | Internal Server Error | `(서버측 에러)`                                      |

<br/>

## Payload result

| Field    | Type       | Description                   |
| -------- | ---------- | ----------------------------- |
| `q`      | `string`   | 쿼리 키워드                   |
| `result` | `string[]` | `q`로 필터된 리스트           |
| `qty`    | `number`   | `q`를 포함한 전체 리스트 길이 |
| `total`  | `number`   | 현제 `result` 길이            |
| `page`   | `number`   | 현재 페이지                   |
| `limit`  | `number`   | per page 사이즈               |

### Sample

```javascript
// Request
`{ GET } https://interview-api.labnote.co}/search?q=lorem&page=1&limit=10`

// RESPONSE (JSON)
{
  "opcode": 200,
  "message": "OK",
  "data": {
      "q": "lorem",
      "result": [
          "Maecenas in lorem sit amet felis volutpat dapibus vulputate at dui.",
          "Nam porta lorem ut turpis pellentesque, et efficitur felis ullamcorper.",
          "Duis fringilla turpis vel lorem eleifend, sit amet hendrerit velit gravida.",
          "Cras in felis eget augue cursus placerat ac eget lorem.",
          "Sed id orci quis mi porttitor pulvinar cursus eget lorem.",
          "Fusce tincidunt lorem ac purus elementum, ut fermentum lacus mollis.",
          "Nam commodo lorem ac posuere dignissim.",
          "Etiam eu elit finibus enim consequat scelerisque aliquam vulputate lorem.",
          "Donec in lorem id eros ornare aliquam ut a nisi.",
          "Donec efficitur nulla eget lorem sollicitudin, in blandit massa dictum."
      ],
      "qty": 10,
      "total": 19,
      "page": 1,
      "limit": 10
  }
}
```

<br/>

---

<br/>

## 💻 로컬 설치 및 실행방법

1. Clone this repo:

```bash
git clone ...
```

2. Install dependencies & packages

```bash
npm i
# OR
yarn
```

3. Run application

```bash
npm run start
# OR
yarn start
```
