# Gulp 
**Gulp**는 자바스크립트 자동화 toolkit이다. 보통 개발환경 띄울때, 빌드할때 사용한다.  
원래 만들어진걸 가져다 쓰는 편이였는데, 이번에 한번 첨부터 만들어 보고자 했다. 그런데.. 이게 보통일이 아니다.
사이트: https://gulpjs.com

## 시작하기.
먼저 아주~~ 간단한 한개의 html파일에 이미지와 스타일만 있는 페이지를 만들었다. 이때 스타일은 css말고 sass로 만들고자 한다. 과정을 글로 적어보자면..
1. html 마크업
2. scss로 스타일 작성
3. scss -> css로 변환
4. 개발 서버 띄워서 확인.

여기서 3번은 sass cli로도 할 수 있다. 4번은 browser-sync로 할 수 있다.그래서 개발환경 띄우는거 자체는 어렵지 않다. 그런데, scss파일을 계속 수정한다면..? 수정 할 때마다 개발환경을 다시 띄워야 한다. 이 얼마나 귀찮은 일인가! **이때 gulp를 사용하면 이걸 자동화 할 수 있다.**

## gulp로 Sass build 하기
먼저 scss -> css로 빌드하는걸 gulp를 이용해서 해보자.  
우선 필요한 패키지를 npm으로 받는다.
```
npm install -D gulp gulp-sass
```


## 코드 
```js
var gulp        = require('gulp');
var browserSync = require('browser-sync').create();
var sass = require('gulp-sass')(require('sass'));
var sourcemaps = require("gulp-sourcemaps");
var fs = require('fs');
var fse = require('fs-extra')


const config = {
  devServer: {
    root: "./dist",
    port: 3000,
    watchList: ["public/**/*.html", "styles/**/*.scss", "styles/**.*.css"],
  },
  src: {
    scss: "./styles/**/*.scss",
    html: "public/index.html",
    images: "public/img",
    js: './js/**/*.js'
  },
  dest: {
    root: "./dist",
    css: "dist/styles/",
    images: "dist/img",
    html: "dist/",
    js: 'dist/js'
  },
};

// Static Server. Promise, resolve를 해야 watch()가 제대로 작동함.
function server() {
  return new Promise((resolve) => {
    browserSync.init({
      server: {
        baseDir: config.devServer.root
      },
      port: config.devServer.port,
      files: config.devServer.watchList,
      directory: true,
      notify: true,
      open: true
    },
      resolve
    );
  })
};

// Compile sass into CSS & auto-inject into browsers
function compileSass() {
    return gulp
      .src("styles/main.scss")
      .pipe(sourcemaps.init())
      .pipe(sass().on("error", sass.logError))
      .pipe(sourcemaps.write())
      .pipe(gulp.dest(config.dest.css));
};

function compileJs() {
  return gulp
    .src(config.src.js)
    .pipe(gulp.dest(config.dest.js));
}

function watch() {
  gulp.watch(config.src.scss, gulp.series(compileSass,compileJs, copyFileToDist))
  gulp.watch(config.src.html, gulp.series(compileSass,compileJs, copyFileToDist))
  gulp.watch(config.src.js, gulp.series(compileSass,compileJs, copyFileToDist));
}

function copyFileToDist() {
  return new Promise((resolve) => {
    fs.copyFileSync(config.src.html, config.dest.html + 'index.html');
    fse.copySync(config.src.images, config.dest.images);
    resolve()
  })
}

function clearDist() {
  return new Promise((resolve) => {
    if (fs.existsSync(config.dest.root)) {
      fs.rmSync(config.dest.root, { recursive: true, force: true })
    }
   
    fs.mkdirSync(config.dest.root);
    fs.mkdirSync(config.dest.css);
    fs.mkdirSync(config.dest.images);
    fs.mkdirSync(config.dest.js);
    
    resolve()
  })
}

module.exports = {
  sass: compileSass,
  start: gulp.series(
    clearDist,
    compileSass,
    compileJs,copyFileToDist,
    server,
    watch
  ),
  build: gulp.series(clearDist, compileSass, compileJs, copyFileToDist),
};
```