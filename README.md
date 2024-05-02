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

