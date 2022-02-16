# AWS Lambda@Edge

## Labda@Edge로 SPA SEO 적용하기.
### 원인
React로 작성한 SPA(Single Page Application)중 일부 경로를 카카오톡, 구글에 공유시 원하는 이미지와 내용이 뜨도록 해달라고 요청이 들어옴.  
OG(Open Graph) 설정이 필요했고, react-helmet -> react-snap 을 시도 해보았으나 [여러 이유들](#reasons)로 인해 안된다는 것을 깨닫고 Lambda@Edge를 시도 해봄.

### 팁
- 작성시점 기준(2022.02.16) Lambda@Edge는 버지니아 북부 리전에서만 사용 가능하다. 근데 cloudwatch에서 실행된 함수의 로그는 cloudwatch를 호출한 리전에 남아있다!! ;ㅅ;.. 이걸 몰라서 여태 업로드 시점의 로그만 보고 하고 있었다.
- 와, facebook과 카카오톡은 og:url경로의 데이터를 불러오는 모양이다. facebook이랑 카톡은 안되고 구글 행아웃은 되길래 도대체 왜이러지 했는데 og:url도 맞게 바꿔주니까 바로 된다. 덕분에 하루종일 삽질했고.. 업그레이드도 했다ㅠㅠ


### <a id="reasons" >다른 방법 실패 이유</a>
#### react-helmet
아주 간단한 이유다. **react-helmet**은 js가 실행되고 meta tag 를 달아준다. Google과 Facebook은 js까지 실행한 후 meta tag값을 가져오기 때문에 문제가 없다.
그러나 카카오톡 네이버등은 html파일만 읽기 때문에 react-helmet으론 대응할 수 없다.  
#### react-snap
그래서 static파일을 자동생성해주는 **react-snap**으로 시도를 했다. 테스트 해본 결과 로컬에서는 잘 되는데, Jenkins 배포 중에 실패를 했다. react-snap은 [puppeteer](https://github.com/puppeteer/puppeteer)를 이용해서 root페이지 부터 나타난 링크들을 모두 돌아보며 해당 링크의 경로에 대한 index.html을 모두 생성한다. 예를들어, 내 메인 페이지에 `/notice`, `/info`, `/contact` 로 가는 링크가 있으면 그걸 모두 읽어서 각각 `/notice/index.html`,`/info/index.html`, `/contact/index.html`을 만들어준다. 아주 편하고 좋다.  
문제는 우리 홈페이지 `/*.html`로 되어 있는 링크들이 몇가지 있었고, 이것들이 jenkins에서 build시 에러를 일으켰다. 이미 스태틱 페이지들이라 따로 스태택페이지를 제작해줄 필요가 없어서 빼려고 했으나, **react-snap 에는 include 옵션은 있어도 exclude 옵션이 없다.** react-snap이 영감을 얻은 react-snapshot에는 exclude옵션이 있지만 이건 deprecate되어서 시도하지 않았다.
#### ~~스태틱 파일 개별 생성~~
react-snap을 사용하는 대신 직접 만들어 줄까 생각해보긴 했지만, 이건 이후에 확장성을 고려하면 최악의 선택지이다.
#### Lambda@Edge
페이지가 s3/cloudfront에 올라가있기 때문에 **Lambda@Edge**라는 선택지가 있었다. (처음엔 전혀 생각 못하다가 팀장님이 말해주셔서 찾아봤다.) Lambda@Edge는
