#### JPA 시작하기 

> ###### 기본 Setting

###### JPA를 사용하기 위한 설정 

```groovy
// gralde.build
implementation 'org.hibernate:hibernate-core:5.6.1.Final'
```

```yaml
# application.yml
jpa:
  hibernate:
    ddl-auto: create
  show-sql: true
  properties:
    hibernate.format_sql: true
```

> ###### 테이블 Mapping 하기 

| value   | descrpition                              |
| :------ | :--------------------------------------- |
| @Entity | 테이블을 매핑한다는것을 JPA에 명시한다                   |
| @Table  | 테이블 정보를 알려준다 - 여러가지 설정을 할수 있으며 <br />@entity 지정시 기본 class명이 들어간다. |
| @Id     | 클래스의 필드를 테이블의 기본키에 매핑한다.<br />식별자 필드라 한다. |
| @Column | 필드를 칼럼에 매핑한다.                            |



> ###### 데이터 베이스 방언 ? 

###### - 데이터 타입 : 가변 문자 타입으로 Mysql은 varchar , Oracle은 varchar2를 사용한다

###### - 다른 함수명: 문자열을 자르는 함수로 SQL 표준은 SUBSTRING 을 사용하지만 오라클은 SUBSTR을 사용한다

###### - 페이징 처리: MYSQL 은 LIMIT 을 사용하지만 Oracle은 Rownum을 사용

---
> ###### JPA로 어플리케이션 개발

```java
SpringApplication.run(OrmApplication.class);
EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("jpabook");
EntityManager entityManager = entityManagerFactory.createEntityManager();
EntityTransaction entityTransaction = entityManager.getTransaction();
try {
	entityTransaction.begin();
	logic(entityManager);
	entityTransaction.commit();
}catch (Exception e){
	entityTransaction.rollback();
}finally {
	entityManagerFactory.close();
}
```

- ###### 엔티티 매니저 팩토리 생성

persistence.xml의 설정 정보를 사용하는 엔티티 매니저 팩토리를 생성해야 한다

엔티티 매니저 팩토리는 어플리케이션 전체에서 한번만 생성하고 공유해서 사용해야 한다.

- 엔티티 매니저 생성

엔티티를 데이터베이스에 등록/수정/삭제/조회할 수 있게한다.

데이터베이스 커넥션풀과 관련이 있으니 스레드간 공유 재사용 금지한다



> 트랜잭션 관리

JPA를 사용하면 트랜잭션 안에서 데이터를 변경해야 한다.

``` java
//등록
em.persist(member);
```

persist 에 저장할 엔티티를 넘겨주면 된다.

```java
//수정
member.setAge(20);
```

update를 할 필요없이 JPA에서 변경을 감지해준다

```java
//한 건 조회
Member findMember = em.find(Member.class, id);
System.out.println("findMember=" + findMember.getUsername() + ", age=" + findMember.getAge());
```

엔티티의 기본키와 매핑한 식별자 값으로 엔티티 하나를 조회하는 가장 단순한 조회 메서드



> JPQL

JPA를 사용하면 어플리케이션 개발자는 엔티티 중심으로 개발하고 데이터베이스에 대한 처리는 JPA에 맡겨야 한다.

- 엔티티 객체를 대상으로 쿼리한다.



