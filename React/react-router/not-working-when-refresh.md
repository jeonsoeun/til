<!-- @format -->

# React Router Not Working When Refresh (React Router 새로고침 할 때 'Cannot GET /route' 가 뜰 때)

## TLDR;

webpack.config.js 에서 아래처럼 `historyApiFallback`옵션을 추가한다.

```javascript
{
  output: {
    devServer: {
      historyApiFallback: true;
    }
  }
}
```

파일을 aws에 올렸다면 404, 403일때 index.html을 호출하도록 cloudfront에서 설정한다.

## Server-side vs Client-side

Server-side rendering 할 때는 server로 request를 보내면 server에서 루트에 맞게 response를 하면 그만이였다.  
그런데 Client-side rendering을 할 때는 만약 `http://example.com` 으로 들어와서 메뉴 클릭을 통해 `http://example.com/about` 으로 이동을 한다면, react-router같은 router 스크립트가 동작하면서 페이지 이동이 아니라 화면을 다시 그려준다. 근데 새로고침을 하거나, 주소를 쳐서 들어온다면? 우선 페이지에 도착한 직후에 router스크립트는 물론 react도 실행되지 않은 상태이기 때문에 브라우저가 server request를 하게되는데 당연히 `http://example.com/about`라는 서버가 없으니까 404오류가 난다.

## 해결

### 1. \#(해시) 라우팅을 한다.(Hash History)

`http://example.com/#/about`  
근본적으로 무조건 index.html을 콜하게 함으로써 문제를 해결하는 방법. \#경로는 서버로 전송하지 않는다.근데 주소가 안이쁘잖아.

### 2. 서버를 만든다.(Isomorphic)

서버를 만들어서 서버로 요청을 보내더라도 작동하게 하자.  
이걸 하자고 서버를 만들자고..????

### 3. 404, 403이 떠도 index.html을 호출하게 설정한다.

이게 제일 괜찮은 방법이다. 404, 403이 뜰때 index.html을 호출하게 해서 index.html을 실행하게 할 수 있다. 호스팅하는 곳에서 설정을 한다(요즘은 자동으로 해주는지 확인을 해봐야 할 것 같다.).  
로컬에선 webpack설정 기준으로 webpack.config.js 에서 아래처럼 `historyApiFallback`옵션을 추가한다.

```javascript
{
  output: {
    devServer: {
      historyApiFallback: true;
    }
  }
}
```

historyApiFallback은 api가 없으면 history를 `/`로 이동시키는 방법이다.

## 참고

https://stackoverflow.com/questions/27928372/react-router-urls-dont-work-when-refreshing-or-writing-manually 에서 일부 발췌. 글이 옛날 글이라 그런지 좀 옛날 느낌이 난다.
