<!-- @format -->

# Clipboard #Copy #복사

js에서 복사를 사용해야 할 때 사용할 수 있는 코드

## navigator.clipboard

```javascript
// 클립보드에 저장
navigator.clipboard.writeText("저장할 String");
```

## window.clipboardData

IE에서 지원해야 될 때. `navigator.clipboard`가 없으면 사용하기.

```javascript
if (!navigator?.clipboard) {
  window.clipboardData.setData("text", "저장할 String");
}
```
