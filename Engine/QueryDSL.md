# QueryDSL
```
QueryDSL은 정적 타입을 이용하여 SQL등의 쿼리를 생성해주는 프레임워크
```

### 장점
1. 문자가 아닌 코드로 쿼리를 작성함으로써, 컴파일 시점에 문법 오류를 쉽게 확인 가능
2. 자동 완성 등 IDE의 도움을 받을 수 있다.
3. 동적인 쿼리 작성이 편하다.
4. 쿼리 작성 시 제약 조건 등을 메서드 추출을 통해 재사용 할 수 있다.

### 단점
- 설정이 까다롭다?

### 부가설명
    컴파일하면 Entity로 등록한 클래스들이 Q클래스로 생성됨(Q쿼리라고도 함)
    RepositoryCustom에서 메소드를 선언해주고
    알맞는 RepositoryImpl에 메소드를 오버라이드해서 쿼리를 구현해주면 됨

### Expressions
    코드 예시
    Expressions.constantAs(bookNo, book.bookNo)
    => 첫번째 인자는 조회 결과에 사용될 변수값 , 두 번째 인자는 alias할 이름을 지정
---
### .Fetch()
1. fetch() : 리스트로 결과를 반환하는 방법(만약 데이터가 없으면 빈 리스트를 반환)
2. fetchOne() : 단건을 조회할 때 사용하는 방법 (결과가 없을때는 null을 반환하고 결과가 둘 이상일 경우 NonUniqueResultException을 던진다.)
3. fetchFirst() : 처음의 한 건을 쿼리해서 가져오고 싶을때 사용
4. fetchResult() : 페이징을 위해 사용될 수 있음. 페이징을 위해 total contents를 가져온다.
5. fetchCount() : count쿼리를 날릴 수 있다.
