---
layout: post
title: "JWT accessToken, refreshToken 적용하기"
subtitle: "JWT"
categories: MOVESTER
tags: BACKEND
comments: true
---

- 목차
  - [1. 왜 토큰을 두 개나 써?](#)
  - [2. refreshToken을 어떻게 사용해?](#)
  - [3. 뭅스터 적용하기](#)
  - [4. 로그아웃 시](#)


<br>

이번에는 지난번 포스팅에 이어, JWT를 활용하여 accessToken과 refreshToken을 관리해보자<br>

## 1. 왜 토큰을 두 개나 써?

JWT 를 통해 토큰을 하나만 발급받아서 사용하면 우린 이것을 accessToken 이라고 부른다.<br>
JWT의 최대 단점은 이 토큰을 제 3자에게 탈취당했을 때, 토큰을 무력화할 수가 없어 보안에 취약하다는 점이다.<br>
그렇기에 accessToken 의 사용 가능 기한은 짧게 주어서 위의 문제점을 해결하고자 하는데,<br>
인증 시간이 짧으니 사용자 입장에서는 로그인을 자주 해줘야해서 불편할 것이다.<br><br>

<mark>"accessToken의 유효기간을 짧게 주면서 사용자도 편한 방법은 없을까?"에서 탄생한 것이 refreshToken이다.</mark><br><br>

refreshToken는 accessToken과 동일하다.<br>
딱 하나 다른 점은 유효기간을 길게 준다는 것이다.<br>
(평균적으로 accessToken-1h refreshToken-14일)<br>

또한 보통 refreshToken 은 서버 db에 저장한다.<br>

<br><br>

## 2. refreshToken을 어떻게 사용해?

사실 accessToken과 refreshToken을 조합하여 위의 문제점을 해결하는 방식은 정말 다양하다.<br>
나는 그 중 구현이 그나마 단순하면서, 위의 문제점은 해결 가능한 방법을 택했다.<br>

1. 사용자 로그인시:  accessToken, refreshToken 발급
2. 로그인한 사용자가 인증이 필요한 api에 요청
3. accessToken 유효 && refreshToken 유효 : api 접근 가능
4. 1시간이 지나 accessToken 만료 && refreshToken 유효 : refreshToken 을 통해 accessToken 재발급
5. 5분뒤, api 요청시, accessToken 유효 && refreshToken 유효 : api 접근 가능
6. 14일이 지난 후 (14일 되기 1시간 전에 4와 같은 상황으로 accessToken은 재발급 받음)
7. accessToken 유효 && refreshToken 만료 : accessToken 을 통해 refreshToken 재발급
8. 또 다시 14일 지난 후 (7 이후 재접속이 없음), accessToken 만료 && refreshToken 만료 : 다시 로그인하도록 요청

<br>

최대한 이해가 쉽도록 작성해보았다.<br>
위의 경우는 총 4개가지의 경우를 다룬 것이다.
- accessToken 유효 && refreshToken 유효 > auth 성공
- accessToken 만료 && refreshToken 유효 > accessToken 재발급 + auth 성공
- accessToken 유효 && refreshToken 만료 > refreshToken 재발급 + auth 성공
- accessToken 만료 && refreshToken 만료 > auth 실패

<mark>핵심은 refreshToken을 사용해서 유효기간이 짧은 accessToken을 재발급한다는 점이다.</mark>


<br><br>

## 3. 뭅스터 적용하기

이제는 코드로 확인해보자.<br><br>


middleware/auth.js
```javascript
const checkToken = async (req, res, next) => {
  if (!req.cookies.accessToken) {
    return res.status(CODE.UNAUTHORIZED).json(form.fail(MSG.UNAUTHORIZED));
  }

  const accessToken = await jwt.verifyAccessToken(req.cookies.accessToken);
  const refreshToken = await jwt.verifyRefeshToken(req.cookies.refreshToken);

  if (!accessToken) {
    if (!refreshToken) {
      // access 만료 refesh 만료
      return res.status(CODE.UNAUTHORIZED).json(form.fail(MSG.UNAUTHORIZED));
    }
    // access 만료 refesh 유효
    const radisToken = await radis.get(refreshToken.idx);

    if (req.cookies.refreshToken !== radisToken) {
      return res.status(CODE.UNAUTHORIZED).json(form.fail(MSG.TOKEN_INVALID));
    }

    const newAccessToken = await jwt.signAccessToken({ idx: refreshToken.idx, email: refreshToken.email });

    res.cookie('accessToken', newAccessToken);
    req.cookies.accessToken = newAccessToken;

    next();
  } else if (!refreshToken) {
    // access 유효 refesh 만료

    const newRefreshToken = await jwt.signRefreshToken({ idx: accessToken.idx, email: accessToken.email });

    radis.set(accessToken.idx, newRefreshToken);
    res.cookie('refreshToken', newRefreshToken);
    req.cookies.refreshToken = newRefreshToken;

    next();
  } else {
    // access 유효 refesh 유효
    req.cookies.idx = accessToken.idx;
    next();
  }
};
```

위에서 보통 refreshToken 을 db에 저장한다고 했는데, 인증을 위해 매번 db에 접근하는 것은 속도가 느려질 수 있어서 나는 인메모리 db인 redis를 활용하였다. (이는 다음 포스팅에서 다루겠다.)<br><br>

코드가 길지만 위에서 설명했던 4가지의 경우로 나누어서 생각하면 간단하다.<br>
한 토큰이 만료됐고, 다른 토큰이 유효하면, 그 토큰을 토대로 만료된 토큰을 새로 발급해주는 내용이다.<br><br>


## 4. 로그아웃 시

로그아웃시에는, 서버에 저장했던 refreshToken 을 지워준다. (db나 redis)<br>
또한 클라이언트 쿠키에 저장된 accessToken과 refreshToken도 지워준다.<br><br>


controller/admin.js
```javascript
const logout = async (req, res) => {
  radis.del(req.cookies.idx);
  res.clearCookie('accessToken').clearCookie('refreshToken').status(CODE.OK).json(form.success(MSG.LOGOUT_SUCCESS));
};
```

