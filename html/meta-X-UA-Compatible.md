<!-- @format -->

# meta tag - X-UA-Compatible

```html
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  ...
</head>
```

## 무슨 용도일까?

**Internet Explorer 의 호환성 보기 설정.**  
Internet Explorer(이하 IE)는 버전별로 렌더링이 차이가 많이난다. 그래서 웹페이지 제공자가 렌더링할 버전을 선택해주는 것이다. 이 값이 설정되어 있지 않다면 사용자가 지정한 호환성 보기 버전을 선택하거나, 마이크로 소프트에서 관리해주는 사이트라면 거기에서 가져온다.  
예를들어 IE 10버전을 기준으로 만들어진 페이지를 IE 11 에서 열면 깨질 수 있기 때문에 ``` html

<meta http-equiv="X-UA-Compatible" content="IE=10" />
``` 
를 head에 넣어준다.

## 설정가능값

- "IE=edge"
- "IE=11"
- "IE=EmulateIE11"
- "IE=10"
- "IE=EmulateIE10"
- "IE=9"
- "IE=EmulateIE9
- "IE=8"
- "IE=EmulateIE8"
- "IE=7"
- "IE=EmulateIE7"
- "IE=5"

## 참고 문서

- https://gocoder.tistory.com/161
- https://stackoverflow.com/questions/6771258/what-does-meta-http-equiv-x-ua-compatible-content-ie-edge-do
