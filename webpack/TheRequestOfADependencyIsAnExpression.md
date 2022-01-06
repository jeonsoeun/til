<!-- @format -->

# critical dependency the request of a dependency is an expression 에러.

### 원인

[Dynamic Requires](https://github.com/webpack/docs/wiki/context#dynamic-require-rewriting)

### 해결

https://stackoverflow.com/questions/37000273/critical-dependencies-the-request-of-a-dependency-is-an-expression-webpack

### TLDR;

웹팩이 경로를 알 수 있도록 `require`안에 변수만으로 경로를 지정해서는 안된다. ex) `require('../' + path)` 이런식으로 해야됨.
