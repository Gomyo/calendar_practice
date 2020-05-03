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


#### :date:2020.05.02
###### [실습]Google Calendar API Application 만들기
Android Google Calendar API 사용법 참조한 진행 [링크](https://solokim.tistory.com/6)
- Step 1: SHA1 인증키
'''
keytool -exportcert -alias androiddebugkey -keystore c:/users/cjw11/.android/debug.keystore -list -v
'''
SHA1: 24:2D:F3:24:EA:9C:17:93:BF:9B:07:F8:1C:B4:7E:AB:10:0A:63:03

- Step 2: 안드로이드 프로젝트 만들기
Minimum API Level : API 28: Android 9.0(Pie)
Package name : com.example.sst_calendar_example (OAuth Client ID 생성에 필요함)

- Step 3: 구글 캘린더 API 활성화
참조 : (https://webnautes.tistory.com/1217)
API 키
OAuth 클라이언트 ID : 567242097103-psfo0n5a6v8iq5cmvvtaiujaqst9h15s.apps.googleusercontent.com

- Step 4: 프로젝트 소스 수정
참조 : (https://developers.google.com/gsuite/guides/android#step_4_prepare_the_project)

- build.gradle(Module: app)의 dependencies 수정: compile말고 implementation 추가. meta-data는 수정하지 않았음

- AndroidManifest 수정 - Add User Permission

- MainActivity.java, xml 파일 복붙
- MainActivity에서 AppcompatActivity 부분을 import class

- Step 5: 내 핸드폰에서 실행해보기
성공했다. Calendertitle이라는 새로운 캘린더를 만들고, 거기에 구글 캘린더 테스트라는 이벤트를 등록할 수 있었다. 이제 옵션 창을 추가하는 것으로 완성할 수 있을 것 같다.

- Step 6: APK 파일 생성
key store path : C:\Coding\sst_calendar\sst_calender_test.jks
pw : 123456

key alias: sttTest
pw : 123456

에러!
Cannot find a version of 'com.google.code.findbugs:jsr305' 
가 떠서 build.gradle(Module:app)에 androidTestImplementation 'com.google.code.findbugs:jsr305:1.3.9' 추가. 그래도 안돼서

'com.google.code.findbugs:jsr305:2.0.1'도 추가하니 정상적으로 빌드됨.

- Step 7: APK로 설치
성공. 다른 사람의 핸드폰에서도 동일하게 기능하는지 확인해 보자.