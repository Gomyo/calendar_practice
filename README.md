# SST_Calendar 구현을 위한 개인 공부 기록:pencil:
#### :date:2020.05.01
###### [생활코딩]구글 API를 통해서 배우는 인증(OAuth 2.0)
**2강 : 동작 매커니즘**
- User가 client에게 id,pw를 주고 client가 필요할 때마다 server에 접근하던 방식은 OAuth에 비해 편리하지만 보안성이 부족하다는 엄청난 단점이 있다.
- 이에 client는 서버에 '당신들 서비스를 사용하겠다'고 등록을 한다. 그러면 서버는 클라이언트에게 id와 secret을 주고 클라이언트는 이를 저장한다 여기서 secret은 절대 공개되면 안된다. 이후 유저가 클라이언트에 접속하면, 익히 보는 '~~한 이유로 우리는 Google의 Calendar 정보가 필요합니다. 그러니까 승인해 주세요.'가 뜨고, 그것을 승인하면 유저는 서버로 바로 접속할 수 있다. 
- 로그인이 되면 서버는 유저에게 '클라이언트가 ~한 정보를 사용하려 하니 승인해 달라'고 하며 scope list와 함께 띄운다. 서버는 그 승인에 대한 정보를 클라이언트에게 Code의 형태로 제공한다. 그러면 클라이언트는 Code, id, secret 세 가지의 정보를 resource server에 다시 보낸다. 그러면 서버는 접속하려는 클라이언트가 유저가 승인한 것이 맞는지 정보를 비교하고, 맞다면 Access Token이라는 중요한 정보를 클라이언트에 제공한다. 클라이언트는 이 액세스 토큰을 사용해 서버에 값을 요청하고, 제공받는다.

**3강 : API 접속하기**
- Google cloud platform를 사용

- 서버에서 ex)google calendar list를 가져오는 작업을 위한 허락을 google과 user 모두에게 알려주는 것을 scope라 칭한다.  

**4강 : oauth 사용하기**
- API를 제한해 두면 id와 pw가 노출되더라도 제공되는 정보가 제한된다.

**5강 : code 획득 절차**
- Access token에는 만료 기간이 있다. 일정 시간이 지나면 클라이언트는 액세스 토큰을 삭제한다.
- 서버의 access type은 2가지가 있는데 online과 offline이 있다.
- offline 방식으로 지정하면 서버가 access token을 줄 때 refresh token을 준다. 액세스 토큰이 만료되면 클라이언트가 서버에게 리프레쉬 토큰을 주면서 새로운 액세스 토큰을 발급받을 수 있다.

- api를 가져올 때 scope= 에 online url encode url을 파라미터로 넣는다.

