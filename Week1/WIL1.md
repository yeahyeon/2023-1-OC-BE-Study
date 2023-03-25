WIL1
======


[Section 0]
-----------
### 개발 환경 설정
1. Java11 설치
2. Intellij 설치
3. Spring 사용: Web Spring, Thymeleaf 사용
4. 접속: localhost:8080 (포트 번호 확인하여 접속)

[Section 1]
-----------
### thymeleaf 템플릿 엔진 동작 방식
1. 웹 브라우저에서 페이지 주소를 입력받으면 내장 톰켓 서버를 거쳐 컨트롤러에게 전달
2. 컨트롤러에서 리턴 값으로 문자를 반환하면 viewResolver가 화면을 찾아서 처리

[Section 2]
-----------
### 정적컨텐츠
**정의**      
서버에서 따로 하는 것 없이 파일을 그대로 웹브라우저에 전달    
**작동 방식**    
웹 브라우저 주소 전달 받으면 내장 톰켓 서버를 거쳐 그대로 html 전달


### MVC와 템플릿 엔진
**정의**        
Model, View, Controller의 약자
* Model : 데이터 전달 역할
* View : 유저에게 화면을 보여주고 컨트롤러와 소통하는 역할    
수업에서 사용한 View는 다음과 같다
```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
```
* Controller : View에서 인풋 값을 받고 데이터를 가공하는 역할     
수업에서 사용한 Controller는 다음과 같다
```java
@GetMapping("hello-mvc")
    public String helloMvc(@RequestParam(value = "name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
```       
          
**작동 방식**     
1. 웹 브라우저가 페이지 주소 입력 받으면 내장 톰켓 서버 거쳐 controller로 전달
2. helloController가 return 값(hello-template) 불러오고 model에서 key(name)와 value(spring) 받아 viewResolver로 전달
3. viewResolver가 template.html과 매핑시켜 변환한 html 웹 브라우저에게 전달   


### API
**json 방식**        
Javascript 객체 문법을 따르는 문자 기반의 데이터 포멧으로, '키-값 쌍'으로 이루어짐
**작동 방식**      
1. 웹 브라우저 주소가 입력되면 내장 톰켓 서버 거쳐 helloController에게 전달
2. @ResponseBody와 같은 어노테이션(소스코드에 추가해서 사용할 수 있는 메타 데이터(컴파일 과정과 실행 과정에서 코드를 어떻게 처리하는지를 알려주기 위한 추가 정보)) 확인
3. @ResponseBody는 HTTP의 body에 문자 내용을 직접 반환함
4. HttpMessageConverter 사용 (문자는 StringConverter, 객체는 JsonConverter)

**API 방식으로 작성한 코드 예시**
```java
@GetMapping("hello-string")
    @ResponseBody //html의 body부분을 직접 내용 넣을 것이다
    public Stping helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
    
@GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
```    


[번외]
------
#### RESTful
**정의**     
REST 원리를 따르는 시스템      
REST란 Representational State Transfer의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미한다.
**구성 요소**               
1. 자원(Resource): 해당 소프트웨어가 관리하는 모든 것(HTTP URI)
2. 행위(Verb): HTTP Method
3. 행위의 내용(Representations): HTTP Message Pay Load
**REST의 특징**                  
1. Uniform(유니폼 인터페이스) : URI로 지정한 리소스에 대한 조작 통일, 한정적인 인터페이스로 수행
2. Stateless(무상태성): 클라이언트의 컨택스트를 서버쪽에 유지하지 않는다.
3. Cacheable(캐시 처리 가능): HTTP 웹표준 그대로 사용
4. Self-descriptiveness(자체 표현 구조): REST API만 보고 쉽게 이해 가능
5. Client-Server Architecture(클라이언트-서버 구조): REST 서버는 API 제공하여 로직 처리 및 저장, 클라이언트는 사용자 인증이나 컨택스트(세션, 로그인 정보) 등을 직접 관리하고 책임짐
**CRUD**                         
소프트웨어가 가지는 기본적인 데이터 처리 기능을 묶어서 일컫는 말. 생성(Create), 읽기(Read), 갱신(Update), 삭제(Delete), header 정보 조회(HEAD)
