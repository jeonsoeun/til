# Syntaxerror unexpected token
## 증상
Create React App으로 App 제작시 react router의 path가 2개 이상일때 발생한다.  
화면에 아무것도 나오지 않으며, 아래와 같은 오류가 나타난다.  
<img src="./syntaxerror-unexpected-token-1.png">  

`Uncaught SyntaxError: Unexpected token '<'`

## 해결
> https://stackoverflow.com/questions/54340240/create-react-app-build-uncaught-syntaxerror-unexpected-token

### TL;DR
package.json에 homepage 옵션을 수정하거나(`homepage: '.'` 또는 `homepage: '페이지 주소'`) 삭제한다.   
참고사항: https://create-react-app.dev/docs/deployment/#building-for-relative-paths

## 원인 
homepage 옵션을 잘못줘서 정적 파일 경로가 잘못되었다. chunkFile을 html로 인식해서 생긴 이슈라고 한다.

## 왜 해결되나..?
내가 package.json의 homepage옵션을 `homepage: './'` 이렇게 주었기 때문에 경로가 잘못 설정된것으로 추청된다.  `homepage: '.'`을 하면 build시에 `The project was built assuming it is hosted at /.`이라고 나타나는데 이게 잘못되어 있던 것으로 추측된다. 왜 추측이냐면.. 지금 뭐가 문제인지 `homepage` 옵션을 바꿔도 적용이 되지 않는다..;;