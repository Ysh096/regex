# 정규 표현식 정리

![regex](./regex.png)

## [강의 영상 ← 클릭](https://youtu.be/t3M6toIflyQ)

공부방법, 사용 예제, 유용한 사이트에 대한 정보는 유튜브 영상에서 확인해 보세요 🙌

연습용 사이트: [regexr.com/5mhou](https://regexr.com/5ml92)

## 문법 정리

### Groups and ranges

| Chracter | 뜻                                                           |
| -------- | ------------------------------------------------------------ |
| `\|`     | 또는                                                         |
| `()`     | 그룹                                                         |
| `[]`     | 문자셋, 괄호안의 어떤 문자든                                 |
| `[^]`    | 부정 문자셋, 괄호안의 어떤 문자가 아닐때 ex) `/[^a-zA-Z0-9]/gm` 문자, 숫자 x |
| `(?:)`   | 찾지만 기억하지는 않음(그룹짓지 않음) ex) `/gr(?:e|a)y/gim`  |

### Quantifiers

| Chracter    | 뜻                                                           |
| ----------- | ------------------------------------------------------------ |
| `?`         | 없거나 있거나 (zero or one)  ex) `/gra?y/` gry 혹은 gray     |
| `*`         | 없거나 있거나 많거나 (zero or more)   ex) `/gra*y/` gry gray graay graaay, ... |
| `+`         | 하나 또는 많이 (one or more)  ex) `/gra+y/` gray graay graaay, ... |
| `{n}`       | n번 반복  ex) `/gra{2}y/` graay                              |
| `{min,}`    | 최소                                                         |
| `{min,max}` | 최소, 그리고 최대  ex) `/gra{1,3}/` gray, graay, graaay      |

### Boundary-type

| Chracter | 뜻                                                           |
| -------- | ------------------------------------------------------------ |
| `\b`     | 단어 경계  ex) `\bYa` 단어 앞에서 쓰이는 Ya만 검색, `Ya\b` 단어 뒤에서 쓰이는 Ya |
| `\B`     | 단어 경계가 아님  ex) `Ya\b` => Ya로 끝나지 않는 **단어**만 검색 |
| `^`      | 문장의 시작  ex) `^Ya` 문장에서 시작하는 Ya                  |
| `$`      | 문장의 끝                                                    |

multiline flag를 사용하지 않으면 `^` 혹은 `$` 사용 시 **문서 전체**의 시작 또는 끝에 있는 문자가 찾는 문자와 일치할 때만 찾아준다.

### Character classes

| Chracter | 뜻                                                 |
| -------- | -------------------------------------------------- |
| `\`      | 특수 문자가 아닌 문자                              |
| `.`      | 어떤 글자 (줄바꿈 문자 제외)                       |
| `\d`     | digit 숫자                                         |
| `\D`     | digit 숫자 아님                                    |
| `\w`     | word 문자                                          |
| `\W`     | word 문자 아님 `/\W/gm` word가 아닌 모든 것을 선택 |
| `\s`     | space 공백                                         |
| `\S`     | space 공백 아님                                    |

만약 `.`이나 `?` 등 정규표현식에서 사용되는 기호 자체를 찾고 싶다면 \를 이용하여 다음과 같이 검색해야 한다.

`/\./gm`  <=  `\` 다음에 찾고자 하는 기호 `.`

`/\?/gm`

예를 들어 `[]` 를 찾고자 하면, `/\[\]/gm`

예를 들어 `{}`를 찾고자 하면, `/\{\}/`

### Flag

global(g) - 매칭되는 다수의 결과값을 기억

multiline(m) - 한줄 단위로 시작과 끝을 구별

case insensitive(i) - 대소문자 구별x



### quiz로 연습하기

1) 010-898-0893 형식 찾기!

`/\d\d\d-\d\d\d-\d\d\d\d/gm`



2) 전화번호 형태 모두 찾기

- 02-878-8888 처럼 앞이 2자리인 경우

- `-`가 아니라 공백이나 `.`으로 번호를 연결하는 경우(010 898 0893, 010.898.0893)

`/\d{2,3}[- .]\d{3}[- .]\d{4}`



3) 이메일 형식 선택하기

`/[a-zA-Z0-9._+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9.]+/gm`

```
dream.coder.ellie@gmail.com
hello@daum.net
hello@daum.co.kr
```

여기서 첫 번째 +는 [a-zA-Z0-9._+-]가 여러 개 있을 수 있다는 것, 그 이후 @가 있어야 함

두 번째 +도 마찬가지이고, 그 이후로 `.`이 있어야 함을 나타냄



4) 유튜브 주소에서 아이디만 가져오기

`/(https?:\/\/)?(www\.)?youtu.be\/([a-zA-Z0-9-]+)/gm`

```
http://www.youtu.be/-ZClicWm0zM
https://www.youtu.be/-ZClicWm0zM
https://youtu.be/-ZClicWm0zM
youtu.be/-ZClicWm0zM
```

해설

- https에서 s는 없어도 됨(? 사용)
- https:// 형식은 없어도 됨(그룹지은 후 ? 사용, `(https?:\/\/)?`)
- www. 형식은 없어도 됨(그룹지은 후 ? 사용, `(www\.)?`)

만약 앞의 https://www. 부분은 제외하고 마지막 그룹 `[a-zA-Z0-9]+`만 그룹지어 기억하고 싶다면

`/(?:https?:\/\/)?(?:www\.)?youtu.be\/([a-zA-Z0-9-]+)/gm`

소괄호 안에 ?: 를 넣는다.



### js에서 정규표현식 사용하기

```js
const regex = /(?:https?:\/\/)?(?:www\.)?youtu.be\/([a-zA-Z0-9-]+)/;
const url = 'https://www.youtu.be/-ZClicWm0zM';
url.match(regex);
```

```
결과
(2) ["https://www.youtu.be/-ZClicWm0zM", "-ZClicWm0zM", index: 0, input: "https://www.youtu.be/-ZClicWm0zM", groups: undefined]
0: "https://www.youtu.be/-ZClicWm0zM"
1: "-ZClicWm0zM"
groups: undefined
index: 0
input: "https://www.youtu.be/-ZClicWm0zM"
length: 2
__proto__: Array(0)
```

배열의 1번 원소에 원하는 그룹이 나옴(그룹이 여러 개면 원소가 여러 개)

```js
const result = url.match(regex);
result[1];

결과
"-ZClicWm0zM"
```



추가적인 공부를 하고 싶다면 [RegexOne](https://regexone.com/)

