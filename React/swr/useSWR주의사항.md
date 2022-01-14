<!-- @format -->

# useSWR 주의사항

## fetcher에서 다중 변수를 요구할 때 잘 적어줘야한다.

백문이불여일견 일단 보자.  
먼저 fetcher 부분이다.

```javascript
const fetchEventLog = (url: string, params: { size: number }) => {
  //...
};
```

이렇게 원하는 변수가 두개 이상일 때, SWR에서는

```javascript
const { data, error, isValidating } = useSWR(
  [`/eventlogs`, { size: LOG_SIZE }],
  fetchEventLog
);
```

위처럼 배열로 묶어서 전달할 수 있다. 물론 다른 방법도 있다.[#](https://swr.vercel.app/ko/docs/arguments) 이때 저 부분이 타입 추론이 안되기 때문에 (내가 못찾은 것 일수도 있지만.. 찾다가 실패했다.) 놓치기가 쉬운데, 놓치면 **어떠한 에러도 없이 그냥 호출이 안된다.**  
처음엔 axios의 문제인가 싶었는데 그것도 아니였고, 한 페이지에서 여러번 호출해서 그런가 했는데 그것도 아니였다. 결론은 두번째 파라미터 값을 useSWR에서 빼먹어서였다;; 타입추론이 너무나 절실하게 필요한거같은데 어떻게 해야될지 모르겠다.
