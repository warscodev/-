# :pushpin: 쿼트위키(quot.wiki)
>**사회적으로 널리 알려진 사람들의 발언 중에서 보존할만한 가치가 있는 말을 등록, 수정, 삭제 할 수 있는 위키입니다.**
>
>https://quot.wiki  또는 구글 '쿼트위키' 검색

</br>



## 1. 제작 기간 & 참여 인원
- 2021년 5월 ~ 현재
- 2인 팀 프로젝트이며 자바&스프링 프레임워크 기반의 풀 스택 웹 개발 업무를 총괄 담당했고,
  전반적인 컨텐츠, 디자인 기획은 팀원과 공동으로 진행했습니다.

</br>



## 2. 사용 기술
#### `Back-end`
  - Java 11
  - Spring Boot, Spring Data JPA, Spring Security, Spring Oauth 2.0 Client 2.4.4
  - QueryDSL 4.4
  - Gradle 6.8
  - H2 1.4.200

#### `DevOps`

- MariaDB 2.7
- AWS EC2, RDS, S3, CodeDeploy, Route 53
- Github Actions
- Nginx

#### `Front-end`
  - Javascript
  - HTML5, CSS, Bootstrap5
  - Thymeleaf

</br>



## 3. ERD 설계

![](https://github.com/warscodev/portfolio/blob/main/%EC%BF%BC%ED%8A%B8%EC%9C%84%ED%82%A4/img/erd.png?raw=true)

## 4. 주요 기능

<details>
<summary><b>펼치기</b></summary>
<div markdown="1">

### 4.1. 발언 등록

- 이용자는 특정 인물을 선택 한 뒤 해당 인물의 발언을 등록할 수 있습니다. 인물이 아직 등록되어 있지 않은 경우에는 인물 등록을 먼저 해야하며 발언을 등록할 때 해당 발언의 일자, 출처 URL, 내용, 관련 태그를 반드시 입력해야합니다. 이는 유효성 검사를 통해 검증되고 조건에 충족하지 않는 경우 메시지를 띄우고 등록이 진행되지 않습니다.

  > - **관련 클래스**
  >
  >   - Remark (Domain Class) :pushpin: [코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/domain/remark/Remark.java)
  >
  >   - RemarkRepository (JPA Interface)
  >   - RemarkApiController :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/web/controller/remark/RemarkApiController.java)
  >   - RemarkService :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/service/remark/RemarkService.java)

  

### 4.2. 발언 목록 및 검색

- 일반 게시판과 피드 방식의 중간 형태로 구현한 발언 목록 페이지에서 카테고리별 발언 보기, 페이지 이동, 좋아요/싫어요, 발언 스크랩이 가능합니다.

- Rest Api 방식으로 사이드에 화제 발언 탭을 구현하여 최근 일주일간 좋아요, 싫어요, 댓글이 가장 많이 달린 순으로 정렬했습니다.

- 사이트 헤더에 구현된 검색창을 통해 사이트 어디서든 검색하여 원하는 발언 또는 인물을 검색할 수 있습니다.

  > - **관련 클래스**
  >
  >   - RemarkContorller
  >   - RemarkService
  >
  >   - RemarkRepositoryImpl (QueryDSL Implements Class) :pushpin: [코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/domain/remark/repository/RemarkRepositoryImpl.java)
  >
  >
  >   - RemarkApiQueryRepository :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/domain/remark/repository/RemarkApiQueryRepository.java)



### 4.3. 댓글

- 개별 발언 페이지에서 로그인 한 회원은 발언에 대한 좋아요/싫어요, 스크랩을 할 수 있고 댓글을 작성할 수 있습니다.

- 댓글에 대한 대댓글을 작성할 수 있고 댓글을 신고할 수 있습니다. 신고된 댓글은 원 내용과 함께 DB에 저장됩니다.

  > - 관련 클래스
  >   - Comment, Reporting (Domain Class)
  >   - CommentApiController :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/web/controller/remark/CommentApiController.java)
  >   - CommentService :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/service/remark/comment/CommentService.java)
  >   - CommentRepositoryImpl :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/domain/remark/comment/repository/CommentRepositoryImpl.java)



### 4.4. 인물 분류 페이지

- 카테고리별 대분류 및 초성별 소분류를 통해 사이트에 등록된 인물들의 통계 정보를 제공하고 <br>개별 인물 페이지로 이동 할 수 있는 인덱스의 역할을 합니다.
  
  > - **관련 클래스**
  >   - PersonController
  >   - PersonService
  >   - PersonRepositoryImpl
  >   - GetChoSungFromNames :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/domain/person/utils/GetChoSungFromNames.java)
  
  
### 4.5. 인물 상세 페이지

- 인물의 개인 정보와 연도별 발언이 노출되고 개인 정보를 편집 할 수 있습니다.

- 로그인 한 회원은 인물을 팔로우 하여 해당 인물의 모든 발언을 모아볼 수 있습니다.

  > - **관련 클래스**
  >   - PersonApiController
  >   - PersonService
  >   - RemarkPersonPageQueryRepository:pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/domain/remark/repository/RemarkPersonPageQueryRepository.java)
  >   - PersonDetail.html :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/resources/templates/person/personDetail.html)



### 4.6. 인물 아이콘 등록 및 관리

- 관리자는 카테고리별 프리셋 아이콘을 등록하여 인물 정보 편집시에 이를 선택하여 등록해 줄 수 있습니다. 아이콘은 AWS S3에 저장됩니다.

- 카테고리별 아이콘을 등록, 수정, 삭제할 수 있고 인물 개별 아이콘을 따로 지정하여 등록 할 수 있습니다.

  >  - **관련 클래스**
  >    - IconApiController
  >    - IconService :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/service/icon/IconService.java)
  >    - IconApiQueryRepository
  >    - AwsS3Config
  >    - S3Uploader :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/config/S3Uploader.java)



### 4.7. Oauth2.0 로그인 및 Spring Security 인증, 인가

- 구글/네이버 Oauth2.0 로그인을 이용해 불필요한 회원 가입 절차 프로세스를 생략했습니다.

- Security 설정을 통해 인증 또는 인가를 받은 회원만 특정 페이지 또는 API에 접근할 수 있도록 제한했습니다.

- Custom Handler, Entry Point로 상황에 따른 추가 작업들을 작성했습니다.

  > - **관련 클래스**
  >   - WebSecurityConfig :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/config/auth/WebSecurityConfig.java)
  >   - CustomOAuth2UserService (OAuth2UserService Implements Class) :pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/config/auth/CustomOAuth2UserService.java)
  >
  >   - CustomAccessDeniedHandler:pushpin:[코드](https://github.com/warscodev/quot/blob/master/src/main/java/com/udpr/quot/config/auth/handler/CustomAccessDeniedHandler.java)
  >
  >   - CustomLoginSuccessHandler
  >
  >   - CustomLogoutSuccessHandler

  

### 4.7. 무중단 배포

- **구조**
![](https://raw.githubusercontent.com/warscodev/portfolio/46244ff960fad61e943617769c98039e1083dff4/%EC%BF%BC%ED%8A%B8%EC%9C%84%ED%82%A4/img/%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%20%E1%84%80%E1%85%AA%E1%84%8C%E1%85%A5%E1%86%BC.svg)
- **과정**

	- Github 마스터 브런치에서 푸시가 발생하면 Github Actions에서 프로젝트 빌드 후, Jar 파일로 압축하여 S3로 업로드 합니다.
	- Github Actions은 CodeDeploy에게 S3로 전달된 Jar 파일을 이용한 배포를 요청합니다.
	- EC2 인스턴스 내부에 있는 CodeDeploy Agent가 S3로 부터 Jar 파일을 전달받아 배포를 진행합니다.
	  - 현재 Nginx와 연결되지 않은 포트의 스프링부트로 배포합니다.
	-  배포와 헬스체크가 완료되면 Nginx reload를 통해 Nginx를 새로 배포된 스프링부트의 포트로 연결합니다.
	
	
	

</div>
</details>

</br>

## 5. 트러블 슈팅
<details>
<summary>npm run dev 실행 오류</summary>
<div markdown="1">

- Webpack-dev-server 버전을 3.0.0으로 다운그레이드로 해결
- `$ npm install —save-dev webpack-dev-server@3.0.0`

</div>
</details>

<details>
<summary> Post 목록 출력시에 Member 객체 출력 에러 </summary>
<div markdown="1">

  - 에러 메세지(500에러)
    - No serializer found for class org.hibernate.proxy.pojo.javassist.JavassistLazyInitializer and no properties discovered to create BeanSerializer (to avoid exception, disable SerializationConfig.SerializationFeature.FAIL_ON_EMPTY_BEANS)
  - 해결
    - Post 엔티티에 @ManyToOne 연관관계 매핑을 LAZY 옵션에서 기본(EAGER)옵션으로 수정

</div>
</details>
    
<details>
<summary> 프로젝트를 git init으로 생성 후 발생하는 npm run dev/build 오류 문제 </summary>
<div markdown="1">

  ```jsx
    $ npm run dev
    npm ERR! path C:\Users\integer\IdeaProjects\pilot\package.json
    npm ERR! code ENOENT
    npm ERR! errno -4058
    npm ERR! syscall open
    npm ERR! enoent ENOENT: no such file or directory, open 'C:\Users\integer\IdeaProjects\pilot\package.json'
    npm ERR! enoent This is related to npm not being able to find a file.
    npm ERR! enoent

    npm ERR! A complete log of this run can be found in:
    npm ERR!     C:\Users\integer\AppData\Roaming\npm-cache\_logs\2019-02-25T01_23_19_131Z-debug.log
  ```

  - 단순히 npm run dev/build 명령을 입력한 경로가 문제였다.

</div>
</details>    

<details>
<summary> JSON: Infinite recursion</summary>
<div markdown="1">


  - 서로 양방향 참조(1:N)를 하고 있는 Remark Entity, Person Entity중 Remark Entity를  JSON으로 반환하는 과정에서 Person Entity가 다시 Remark Entity를 불러오는 무한 순환참조 문제가 발생.

  - 양 Entity에 각각 @JsonManagedReference, @JsonBackReference을 붙여서 순환참조를 방어할 수도 있으나 Entity 자체를 반환하기 보다는 dto 객체를 만들어 필요한 필드들만 추출해 반환한다면 위 문제가 발생하지 않습니다. 

    

</div>
</details>  

<details>
<summary> H2 접속문제</summary>
<div markdown="1">

  - H2의 JDBC URL이 jdbc:h2:~/test 으로 되어있으면 jdbc:h2:mem:testdb 으로 변경해서 접속해야 한다.
        

</div>
</details> https://element.eleme.io/#/en-US/component/quickstart#global-config)



<details>
<summary> HTTP delete Request시 개발자도구의 XHR(XMLHttpRequest )에서 delete요청이 2번씩 찍히는 이유</summary>
<div markdown="1">

  - When you try to send a XMLHttpRequest to a different domain than the page is hosted, you are violating the same-origin policy. However, this situation became somewhat common, many technics are introduced. CORS is one of them.

        In short, server that you are sending the DELETE request allows cross domain requests. In the process, there should be a **preflight** call and that is the **HTTP OPTION** call.

        So, you are having two responses for the **OPTION** and **DELETE** call.

        see [MDN page for CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS).

    - 출처 : [https://stackoverflow.com/questions/35808655/why-do-i-get-back-2-responses-of-200-and-204-when-using-an-ajax-call-to-delete-o](https://stackoverflow.com/questions/35808655/why-do-i-get-back-2-responses-of-200-and-204-when-using-an-ajax-call-to-delete-o)
      

</div>
</details> 

<details>
<summary> 이미지 파싱 시 og:image 경로가 달라서 제대로 파싱이 안되는 경우</summary>
<div markdown="1">

  - UserAgent 설정으로 해결
        - [https://www.javacodeexamples.com/jsoup-set-user-agent-example/760](https://www.javacodeexamples.com/jsoup-set-user-agent-example/760)
        - [http://www.useragentstring.com/](http://www.useragentstring.com/)
      

</div>
</details> 
    
<details>
<summary> 구글 로그인으로 로그인한 사용자의 정보를 가져오는 방법이 스프링 2.0대 버전에서 달라진 것</summary>
<div markdown="1">

  - 1.5대 버전에서는 Controller의 인자로 Principal을 넘기면 principal.getName(0에서 바로 꺼내서 쓸 수 있었는데, 2.0대 버전에서는 principal.getName()의 경우 principal 객체.toString()을 반환한다.
    - 1.5대 버전에서 principal을 사용하는 경우
    - 아래와 같이 사용했다면,

    ```jsx
    @RequestMapping("/sso/user")
    @SuppressWarnings("unchecked")
    public Map<String, String> user(Principal principal) {
        if (principal != null) {
            OAuth2Authentication oAuth2Authentication = (OAuth2Authentication) principal;
            Authentication authentication = oAuth2Authentication.getUserAuthentication();
            Map<String, String> details = new LinkedHashMap<>();
            details = (Map<String, String>) authentication.getDetails();
            logger.info("details = " + details);  // id, email, name, link etc.
            Map<String, String> map = new LinkedHashMap<>();
            map.put("email", details.get("email"));
            return map;
        }
        return null;
    }
    ```

    - 2.0대 버전에서는
    - 아래와 같이 principal 객체의 내용을 꺼내 쓸 수 있다.

    ```jsx
    UsernamePasswordAuthenticationToken token =
                    (UsernamePasswordAuthenticationToken) SecurityContextHolder
                            .getContext().getAuthentication();
            Map<String, Object> map = (Map<String, Object>) token.getPrincipal();
    
            String email = String.valueOf(map.get("email"));
            post.setMember(memberRepository.findByEmail(email));
    ```
    

</div>
</details> 
    
    

</div>
</details> 
    
</br>

