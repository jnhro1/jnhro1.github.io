---
layout: post
title: "[OAuth] 02. OAuth-kakao"
subtitle: "OAuth"
categories: study
tags: study
comments: true
date: "2021-05-06"
related_posts:
- category/_posts/study/2021-05-06-OAuth.md
published: true
---

- 목차
  - [1. Movester-kakao 등록하기](#.Movester-kakao-등록하기)
  - [2. passport 모듈 이용하기](#.passport-모듈-이용하기)
<br><br>

  나는 보다 사회적인 애플리케이션을 만들기 위해 movester에 소셜 로그인을 적용하고자 하였는데, 과연 어떤 소셜로그인을 적용시킬까에 대한 고민이 들었다.
<br><br>
  초기에는 "다다익선 아닐까?" 하며, 로컬, 구글, 카카오, 페이스북, 네이버를 통한 로그인을 모두 구현하려 했지만 각 서비스에 의존할수록 그 서비스에 바로 대응할 수 있도록 준비를 하고 있어야 한다는 제약이 생겨 카카오 소셜 로그인만 구현하고자 결정하였다.
<br><br>
  다양한 소셜 로그인 중 카카오를 선택한 이유는 movester 서비스가 현재 카카오 오픈채팅방을 통해 매일 스트레칭 알람을  보내고 있고, 향후 서비스를 확장할 때에도 카카오 플랫폼을 통해 확장해 갈 예정이기 때문이다.
<br><br>
  카카오 소셜 로그인에 대한 카카오 공식 문서는 다음과 같다.

  [Kakao Developers](https://developers.kakao.com/docs/latest/ko/kakaologin/common)

## 1. Movester-kakao 등록하기

  지난 포스트 2번에 다룬 Movester의 등록 과정이다.

  카카오 소셜 로그인을 사용하기 위해선 카카오에 Movester를 등록해야한다.

  ![oauth](/assets/img/study/카카오1.png)

  [https://developers.kakao.com/](https://developers.kakao.com/)

  카카오 디벨로퍼 사이트에 들어가서 "시작하기"를 누른다.
  <br><br><br><br>

  ![oauth](/assets/img/study/카카오2.png)

  "애플리케이션 추가하기" 에 들어간다.
  <br><br><br><br>

  ![oauth](/assets/img/study/카카오3.png)

  등록할 앱 이름과 사업자명을 작성한 뒤 "저장"을 클릭한다.
  <br><br><br><br>

  ![oauth](/assets/img/study/카카오4.png)

  movester는 웹 기반 플랫폼이므로 "Web 플랫폼 등록"을 클릭한다.
  <br><br><br><br>

  ![oauth](/assets/img/study/카카오5.png)

  사이트 도메인을 입력한다.

   아직 aws를 통한 도메인 등록을 하기 전이므로 [http://localhost:5000으로](http://localhost:5000으로) 등록하였다.
   <br><br><br><br>

  ![oauth](/assets/img/study/카카오6.png)

  도메인 등록후 Redirect Uri 등록을 위해 아래의 "등록하러 가기"를 클릭한다.
  <br><br><br><br>

  ![oauth](/assets/img/study/카카오7.png)

  카카오 로그인 off 버튼을 클릭하여 on 상태로 변경하여준다.

  "Redirect URI 등록" 버튼을 클릭한다.
  <br><br><br><br>

  ![oauth](/assets/img/study/카카오8.png)

  Redirect URI를 등록한다. 나는 [http://localhost:5000/auth/kakao/callback/](http://localhost:5000/auth/kakao/callback/) 으로 지정하였다.

  위의 리다이렉트 주소는 지난 포스팅에서 언급한 authorized code를 전달받을 주소이다.
  <br><br>  <br><br>

  ![oauth](/assets/img/study/카카오9.png)

  이제 Client Secret을 발급받자.

  이 토큰은 절대 타인에게 공유하면 안되는 내용이니 보안에 유의하도록 하자.

  자. 위의 과정을 거쳐 우리는 movester를 카카오 api에 등록하였다.

  위 과정을 통해 우리는

> client_id

> client_secret

> Authorized redirect URIs

  를 카카오와 공유하게 되었다.

## 2.  passport 모듈 이용하기

  지난 포스팅 마지막에 우린 OAuth를 정확히 알지 못해도 활용할 수 있는 다양한 라이브러리가 존재한다고 언급하였다. 실제로 OAuth의 전 과정을 라이브러리 도움 없이 내가 직접 구현하려면 그 과정이 매우 복잡하므로 우린 node에서 제공하는 passport 모듈을 사용하자.

  passort-kakao 의 공식문서이다.

  [passport-kakao](http://www.passportjs.org/packages/passport-kakao/)

  ```jsx
  npm install passport-kakao
  ```

  해당 명령어를 통해 passport-kakao를 설치한다.

### config/auth.js

  ```jsx
  const kakaoKey = {
  	clientID: process.env.KAKAO_CLIENT_ID,
  	clientSecret: "",
  	callbackURL: "http://localhost:5000/auth/kakao/callback/" };
  ```

  config 파일에는 db나 다른 라이브러리와 통신을 위한 id, pw 같은 식별값을 넣어두는 공간이다.

  인증을 위한 auth.js 파일을 만들어 위에서 작업한 카카오와의 약속된 키값들을 넣자.

  참고로 나는 dotenv 모듈을 통해 키값들을 변수로 선언하여 관리하였다.

### routes/auth.js

  ```jsx
  const KakaoStrategy = require("passport-kakao").Strategy;

  passport.use(
    "kakao-login",
    new KakaoStrategy(kakaoKey, (accessToken, refreshToken, profile, done) => {
      console.log(profile);
    })
  );

  app.get(
      '/auth/kakao',
      passport.authenticate('kakao')
    );

  app.get(
      '/auth/kakao/callback',
      passport.authenticate('kakao', { failureRedirect: 'auth/login' }),
      function (req, res) {
        res.redirect('/');
      },
    );
  ```

  passport는 카카오, 구글, 페이스북 등 각각의 OAuth 활용 모듈을 Strategy. 즉 전략으로 제공한다.

  카카오 소셜로그인 api 전략을 require 받는다.

  passport.use를 통해 config/auth.js에서 정의한 kakaokey를 바탕으로  accessToken, refreshToken, 사용자 profile 을 받는다.

  즉 이 과정은 전 포스팅의 User의 승인, Their의 승인 과정이다.

  이제 라우터를 만들어서 movester와 카카오의 길을 연결해줘야한다.

  또한 리다이렉트 콜백을 만들어서 성공시 authorization_code를 받을 주소와 실패시 실패여부를 받을 곳을 지정해준다.

  [http://localhost:5000/auth/kakao](http://localhost:5000/auth/kakao) 에 접속하자.

  (사진 추후 업로드 예정)

  우리가 익히 아는 카카오 로그인 화면이 나올 것이다.

  사용자가 id, pw를 입력해 로그인을 하면, 카카오에서는 movester에 표시된 정보 제공을 허용하겠느냐 라는 화면이 나온다.

  사용자가 동의를 하면, kakao는 Movester에게

  ```jsx
  { provider: 'kakao',
    id: 947918234,
    username: '조나현',
    displayName: '조나현',
    _raw:
     '{"id":947918234,"uuid":"d0R1RnZAdEBxXWxbYlNrWW9baER2RXxOd0cv","properties":{"profile_image":null,"nickname":"신동규","thumbnail_image":null}}',
    _json:
     { id: 947918234,
       uuid: 'd0R1RnZAdEBxXWxbYlNrWW9baER2RXxOd0cv',
       properties:
        { profile_image: null, nickname: '조나현', thumbnail_image: null } } }
  ```

  과 같은 profile 정보를 넘겨준다.

  이제 우리는 이 정보를 통해 회원가입/로그인 기능을 구현하면 된다.


<br>
  이처럼 우리는 passport라는 모듈을 통해 OAuth의 복잡한 과정을 직접 구현하지 않고 이용할 수 있다.

