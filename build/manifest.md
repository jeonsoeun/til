# Manifest
## manifest.json [21.11.08]
페이지를 build할때 만약 결과물이 `bundle.js` 라고 한다면 `bundle.3fe432.js` 이런식으로 빌드를 한 뒤 웹서버에서 호출할때 저 파일을 불러오도록 한다면, 캐시문제를 해결할 수 있다.   
다만, 문제는 저 중간에 해시값이 계속 바뀔텐데 그걸 매번 웹서버에서 하드코딩해주는건 너무 번거로운 일이다. 그래서 그걸 해결하기 위해 `manifest.json`을 만들고 내용물은 이런식으로 구성된다.
```json
{
  "js": "bundle.3fe432.js",
  "css": "bundle.652ead.css"
  ...
}
```
그리고 웹서버에서 `manifest.json`을 읽어서 js파일 이름을 가져온 다음 그걸 열도록 하면 캐시문제도, 하드코딩이슈도 한번에 해결된다. 