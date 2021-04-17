리액트는 정말 잘 만든 프레임워크다! 그 오묘함에(주로 implementation에) 놀라곤 한다.

리액트 네이티브 프로젝트를 진행하고 있는데, Route와 Controller를 분리하는 것이 꽤나 흥미로웠다.
근데 누군 URL을 접근할 수 있었고, 실제 Postman으로도 너무 쉽게 접근할 수 있었다.
그래서 보안을 한 번 달아보기로 했다.

Cognitoㄹ 로그인하면, User Token과 Access Token을 Return한다
당연히 access token으로 해야하지만, 베타테스트가 코앞이라 userId로 우선 급한대로 틀어막았다.
Access Token으로 인증하는 법은 추후에 다룰 예정이다.

Cognito로 로그인하는데는, 우리 서버가 필요하지 않다. 이 점을 이용하여
1) 로그인 완료
2) AsyncStorage에 토큰을 저장
3) 토큰으 Axios default로 넘겨주는 형태를 취했다.

```
try{
  const token = await AsyncStorage.getItem("UserToken");
  console.log(token);
  API.defaults.headers.common['token'] = token; //login시 이미 불렸기 때문에 api에 대해 한번 call 해줍니다
  axios.defaults.headers.common['token'] = token; //이후의 api call을 모두 변경합니다
} catch(err) {
  throw err;
}
```
띠용? 왜 defaults를 두번 부를까?
우리 프로젝트는 API를 모두 코드 분리해 놓았고, UseCallback으로 부른다.
이 안에 axios.create를 넣어둬서, 코드최적화를 꾀했다.

```
사실 이제 알았는데 usecallBack으로 불렀는데, console.log가 계속 찍힌다. 코드분리를 하면서 문제가 생긴 모양이다.
```
그래서 현재 만들어진 API에 대한 default를, 그릭 추후에 있을 모든 axios에 대한 default를 설정하는 것이다.

그 이후 Preinterceptor로 설정해주면 된다(혹은 설정하지 않아도 된다)
```
'token' header를 await AsyncStorage.getItem("UserToken")
```
중요한 PostInterceptor
```
const post_intercept = async (axios_obj) => {
    axios_obj.interceptors.response.use(
        function (response) {
          //then() 으로 이어집니다
            return response;
        },
    
        function (err) {
          //catch으로 이어집니다
            const errmsg = err.response.data.message;
            if(errmsg == "what I want")
                    alert("안전하지 않은 접속입니다. 다시 로그인해주세요"); 
          return Promise.reject(err);
        }
    );
}

```
이런 식으로 짠다니 잘 작동한다!
Access Token은 현재 프로젝트 DB에 저장할 예정인데, 혹시 Cognito에 이를 비교하는 기능이 있다면, 이를 확인할 예정이다!
