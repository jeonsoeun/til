# S3 정적 호스팅
s3를 이용해서 단일 페이지를 호스팅 할 일이 있고, 굳이 CDN과 이쁜 도메인이 필요하지 않다면 S3만을 이용해서 정적 호스팅을 할 수 있다.  

### 준비물
AWS 계정 생성 및 로그인 완료, S3 버킷 생성완료, 만든 S3버킷에 index.html파일 추가

1. 먼저 원하는 파일을 원하는 버킷에 업로드 한다.
2. 해당 파일이 있는 버킷에 들어가서 권한을 누른다.
    ![권한 누르기]('./s3-staic-hosting-1.png')
3. 호스팅을 하기 위해서는 당연히 해당 버킷이 퍼블릭 권한을 얻어야 한다. 초기에 버킷설정을 번경하지 않았다면 아래 이미지 처럼 되어있을텐데
    ![퍼블릭 액세스 설정 전]('./s3-staic-hosting-2.png')
    아래 스크린샷처럼 바꿔준다.
    ![퍼블릭 액세스 설정 후]('./s3-staic-hosting-3.png')
4. 그리고 그 밑에 버킷정책 설정을 해주어야 한다. 외부에서 접근했을때 버킷에 있는 파일들에 접근 할 수 있어야 호스팅을 할 수 있다. 그래서 `GetObject`권한을 추가하려한다.
    '편집'버튼을 누르고,
    ![버킷정책 설정]('./s3-static-hosting-4.png')
    나타난 화면에서 정책 생성기를 클릭한다.
    ![정책생성기]('./s3-static-hosting-5.png')
    당연히 처음엔 위에 두 화면 모두에 아무것도없을 것이다.
5. 정책생성기에서 먼저 S3 버킷 설정이니 선택하고,
    ![AWS Policy Generator - Step1]('./s3-static-hosting-6.png')
    아래에서 다음과 같이 'Principal'과 'Actions'설정을 한다.
    ![AWS Policy Generator - Step2]('./s3-static-hosting-7.png);
    그리고 ARN 값은 이전 '버킷 정책 설정' 페이지로 돌아가서 `Resource:` 뒤에 적힌것을 복사해서 붙여넣기한다.
    ![AWS Policy Generator - Step2 - ARN]('./s3-static-hosting-8.png')
6. 'Add Statement'를 누르고 내용을 확인한 뒤 'Generate Policy'를 누른다.
    ![AWS Policy Generator - Step3]('./s3-static-hosting-9.png')
7. 이제 '속성'탭으로 이동해서 제일 하단에 '정적 웹 사이트 호스팅'하단에 주소를 열어본다.
    ![정적 웹 사이트 호스팅]('./s3-static-hosting-10.png')
    웹사이트가 잘 뜨면 끝!! 이 주소가 정적 웹사이트 호스팅 주소가 되시겠다. 
8. 만약 안뜨고 `Access Denied`오류가 뜬다면 십중팔구 아마 index.html파일의 권한이 이상할것이다. 그러면 '객체'탭에서 index.html을 누르고 '권한'탭으로 들어간다. 그리고 거기에서 'ACL(액세스 제어 목록)'을 수정해, '모든 사람(퍼블릭엑세스)'에 읽기 권한을 추가해준다.
    ![권한 설정]('./s3-static-hosting-11.png')
    버킷에 추가되는 파일들이 모두 퍼블릭 엑세스를 받도록 하는 설정이 어디 있는거같긴한데.. 못찾았따..ㅠ
