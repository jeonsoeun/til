# Array.reducer()
## Array.reducer((accumulator[, currentValue, currentIndex, array])=>{} [, initialValue])
accumulator는 누산기로, 이전값과 현재값을 데이터로 받는다. 이때 첫번째 요소의 경우 이전값으로 콜백의 이전 반환값 또는 initialValue가 있는경우 initialValue이다.
```
accumulator = (previousValue, currentValue) => {...}
```

## 사용
누적계산 또는 배열값을 이어 붙이기 등을 하는경우에 쓰면 되겠다.