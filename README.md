# 입력 폼 처리

##### th:object  : 커맨드 객체를 지정한다.
##### *{...} : 선택 변수 식이라고 한다. th:object 에서 선택한 객체에 접근한다
##### th:field
- HTML 태그의 id , name , value 속성을 자동으로 처리해준다.

##### 렌더링 전
```
 <input type="text" th:field="*{itemName}" />
```

##### 렌더링 후 
```
<input type="text" id="itemName" name="itemName" th:value="*{itemName}" />
```

# 체크 박스 - 단일

HTML checkbox는 선택이 안되면 클라이언트에서 서버로 값 자체를 보내지 않는다. 수정의 경우에는 상황에 따라서
이 방식이 문제가 될 수 있다. 사용자가 의도적으로 체크되어 있던 값을 체크를 해제해도 저장시 아무 값도 넘어가지 않
기 때문에, 서버 구현에 따라서 값이 오지 않은 것으로 판단해서 값을 변경하지 않을 수도 있다.

이런 문제를 해결하기 위해서 스프링 MVC는 약간의 트릭을 사용하는데, 히든 필드를 하나 만들어서, _open 처럼 기존
체크 박스 이름 앞에 언더스코어( _ )를 붙여서 전송하면 체크를 해제했다고 인식할 수 있다. 히든 필드는 항상 전송된
다. 따라서 체크를 해제한 경우 여기에서 open 은 전송되지 않고, _open 만 전송되는데, 이 경우 스프링 MVC는 체크
를 해제했다고 판단한다.

```
<input type="checkbox" id="open" name="open" class="form-check-input">
<input type="hidden" name="_open" value="on"/> <!-- 히든 필드 추가 -->
<label for="open" class="form-check-label">판매 오픈</label>
```

##### 체크 박스 체크
- open=on&_open=on
 - 체크 박스를 체크하면 스프링 MVC가 open 에 값이 있는 것을 확인하고 사용한다. 이때 _open 은 무시함
##### 체크 박스 미체크
- _open=on
 - 체크 박스를 체크하지 않으면 스프링 MVC가 _open 만 있는 것을 확인하고, open 의 값이 체크되지 않았다고 인식함

# 체크 박스 - 단일2

```
 <div class="form-check">
 <input type="checkbox" id="open" th:field="*{open}" class="form-checkinput">
 <label for="open" class="form-check-label">판매 오픈</label>
 </div>
```
th:field="*{open}" 를 추가하면  <input type="hidden" name="_open" value="on"/> 를 자동적으로 생성 해준다.

##### 타임리프의 체크 확인
- checked="checked"
체크 박스에서 판매 여부를 선택해서 저장하면, 조회시에 checked 속성이 추가된 것을 확인할 수 있다. 이런 부분을
개발자가 직접 처리하려면 상당히 번거롭다. 타임리프의 th:field 를 사용하면, 값이 true 인 경우 체크를 자동으로
처리해준다.

# 체크 박스 - 멀티

##### @ModelAttribute의 특별한 사용법

- 공통적인 데이터를 model.addAttribute로 보내준다면 @ModelAttribute를 통해 컨트롤러 요청 할때마다 model 담기게 해준다. 각각 메서드 마다 model.addAttribute를 구성을 안해주고 공통으로 @ModelAttribute 통해 넣어준다.

```
<div>
 <div>등록 지역</div>
 <div th:each="region : ${regions}" class="form-check form-check-inline">
 <input type="checkbox" th:field="*{regions}" th:value="${region.key}"
class="form-check-input">
 <label th:for="${#ids.prev('regions')}"
 th:text="${region.value}" class="form-check-label">서울</label>
 </div>
</div>
```

- th:for="${#ids.prev('regions')}
- HTML의 id 가 타임리프에 의해 동적으로 만들어지기 때문에 <label for="id 값"> 으로 label 의 대상이 되는
id 값을 임의로 지정하는 것은 곤란하다. 타임리프는 ids.prev(...) , ids.next(...) 을 제공해서 동적으로
생성되는 id 값을 사용할 수 있도록 한다.


