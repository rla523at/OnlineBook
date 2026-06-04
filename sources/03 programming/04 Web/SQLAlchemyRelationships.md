# SQLAlchemy Relationships

## 한 줄 요약

SQLAlchemy relationship은 DB table 사이의 연결을 Python 객체 탐색으로 다루기 위한 ORM 설정이고, ForeignKey는 DB schema에 남는 참조 제약이다.

## 이 문서의 목표

이 문서는 [DatabaseAndORM.md](./DatabaseAndORM.md)를 읽은 뒤 ORM 관계 매핑을 더 자세히 이해하려는 독자를 위한 심화 문서다.

다음 내용을 다룬다.

- `ForeignKey`와 `relationship()`의 역할 차이
- one-to-many와 many-to-many 관계를 SQLAlchemy에서 표현하는 방법
- link table, `back_populates`, `cascade`의 의미
- eager loading, `selectinload()`, N+1 query의 관계

## 먼저 알아야 할 용어

- primary key
  - table 안에서 row 하나를 구분하는 기준 column이다.
- foreign key
  - 다른 table의 primary key를 참조하는 column 또는 제약 조건이다.
- relationship
  - SQLAlchemy ORM에서 관련 객체로 이동할 수 있게 만드는 Python 속성 설정이다.
- loading strategy
  - ORM 관계에 연결된 객체를 언제 어떤 query로 읽을지 정하는 조회 방식이다.

## ForeignKey와 relationship

`ForeignKey`는 DB table 사이의 참조 제약이고, `relationship()`은 Python 객체 사이의 탐색 경로다.

```python
class Comment(DbBaseModel):
    article_id = Column(Integer, ForeignKey('articles.id', ondelete='CASCADE'), nullable=False)
    article = relationship('Article', back_populates='comments')
```

이 예시는 다음처럼 나눠 볼 수 있다.

- `article_id`
  - DB에 실제로 저장되는 column이다.
- `ForeignKey('articles.id')`
  - `comments.article_id`가 `articles.id`를 참조한다는 DB 제약이다.
- `article`
  - Python 코드에서 `comment.article`처럼 관련 객체로 이동하기 위한 ORM 속성이다.

ForeignKey만 정의하면 DB 수준의 참조 규칙은 생긴다. 하지만 Python 코드에서 `comment.article`처럼 객체를 탐색하려면 `relationship()` 설정이 필요하다.

## one-to-many

one-to-many는 한쪽 row 1개가 다른쪽 row 여러 개와 연결되는 관계다. 하나의 게시글에 여러 댓글이 달리는 관계가 이 조건에 맞는다.

```text
Article 1 -> Comment N
```

```python
class Article(DbBaseModel):
    comments = relationship(
        'Comment',
        back_populates='article',
        cascade='all, delete-orphan',
    )

class Comment(DbBaseModel):
    article_id = Column(Integer, ForeignKey('articles.id', ondelete='CASCADE'), nullable=False)
    article = relationship('Article', back_populates='comments')
```

코드에서는 다음처럼 양쪽 방향으로 이동할 수 있다.

```python
article.comments
comment.article
```

`Comment.article_id`는 DB에 저장되는 값이고, `Article.comments`와 `Comment.article`은 SQLAlchemy가 Python 객체 탐색을 위해 제공하는 속성이다.

## many-to-many와 link table

many-to-many는 양쪽 모두 여러 row와 연결될 수 있는 관계다. 학생과 강의는 다음 조건에서 many-to-many 관계로 표현할 수 있다.

- 한 학생은 여러 강의를 수강할 수 있다.
- 하나의 강의에는 여러 학생이 등록될 수 있다.

관계형 DB는 many-to-many 연결을 한 column만으로 직접 저장하지 않는다. 그래서 양쪽 table의 primary key를 함께 저장하는 중간 table을 둔다. 이 문서에서는 이 중간 table을 link table이라고 부른다.

```python
class StudentCourseEnrollment(DbBaseModel):
    __tablename__ = 'student_course_enrollments'

    student_id = Column(Integer, ForeignKey('students.id', ondelete='CASCADE'), primary_key=True)
    course_id = Column(Integer, ForeignKey('courses.id', ondelete='CASCADE'), primary_key=True)
```

SQLAlchemy에서는 추가 의미가 없는 순수 연결 table을 내부 구현처럼 감추고 객체에서는 목록처럼 다룰 수 있다.

```python
class Student(DbBaseModel):
    courses = relationship(
        'Course',
        secondary='student_course_enrollments',
        back_populates='students',
    )

class Course(DbBaseModel):
    students = relationship(
        'Student',
        secondary='student_course_enrollments',
        back_populates='courses',
    )
```

```python
student.courses
course.students
```

단, link table에 `enrolled_at`, `status`, `grade` 같은 추가 column이 있다면 단순 연결 table이 아니라 별도 도메인 객체로 다루는 선택지가 있다. 이 경우 enrollment 자체가 저장해야 할 의미를 갖기 때문이다.

## back_populates

`back_populates`는 relationship의 양쪽 끝을 서로 연결한다. 한쪽 객체에서 다른쪽 객체로 이동하는 속성과 반대 방향 속성이 같은 관계임을 ORM에게 알려준다.

```python
Article.comments <-> Comment.article
Student.courses <-> Course.students
```

양쪽에 `back_populates`를 맞춰 두면 SQLAlchemy가 양방향 관계를 일관되게 관리할 수 있다.

예를 들어 `article.comments.append(comment)`처럼 한쪽에서 관계를 연결했을 때, 같은 Session 안에서 `comment.article` 방향도 같은 관계로 이해할 수 있다.

## cascade

`cascade='all, delete-orphan'`은 하위 객체가 부모 객체 없이는 의미를 갖기 어려운 관계에서 사용할 수 있다.

```python
class Article(DbBaseModel):
    comments = relationship(
        'Comment',
        back_populates='article',
        cascade='all, delete-orphan',
    )
```

이 설정은 다음 의미를 갖는다.

- 부모인 `Article`이 삭제될 때 관련 `Comment`도 함께 정리된다.
- 어떤 `Comment`가 더 이상 어떤 `Article`에도 속하지 않으면, 어느 부모에도 연결되지 않은 고아 row로 남기지 않는다.

`cascade`는 DB의 `ON DELETE CASCADE`와 같은 계층의 설정이 아니다. `cascade`는 SQLAlchemy ORM이 객체 작업을 처리할 때 적용하는 규칙이고, `ON DELETE CASCADE`는 DB가 foreign key 제약을 처리할 때 적용하는 규칙이다.

## eager loading과 selectinload

`relationship()`은 관계 자체를 정의한다. 관련 객체를 언제 읽을지는 별도 문제다.

eager loading은 관련 객체를 나중에 하나씩 읽지 않고, 처음 조회할 때 함께 읽도록 지정하는 조회 전략이다.

```python
select(Article).options(
    selectinload(Article.comments),
)
```

`selectinload()`는 SQLAlchemy의 eager loading 방식 중 하나다. 이 코드는 게시글 목록을 가져올 때 각 게시글의 댓글도 별도 query로 미리 읽어 두라는 의미다.

관계를 정의하는 것이 `relationship()`이라면, 조회 시점에 loading strategy를 정하는 것이 `selectinload()` 같은 option이다.

## N+1 query

N+1 query는 목록을 조회하는 query 1번 뒤에, 목록의 각 row마다 관련 객체를 읽는 query가 반복되는 상황을 뜻한다.

```text
Article 목록 조회 1번
  -> Article 1의 comments 조회
  -> Article 2의 comments 조회
  -> Article 3의 comments 조회
  -> ...
```

게시글 100개를 가져온 뒤 각 게시글의 댓글을 따로 읽으면 query 수가 빠르게 늘어난다. 이 상황에서 댓글 목록을 함께 보여줘야 한다면 `selectinload(Article.comments)` 같은 eager loading 전략을 검토한다.

## 자주 하는 실수

- `ForeignKey`만 있으면 Python 객체 관계가 자동으로 생긴다고 생각하는 경우
- `relationship()`만 있으면 DB 참조 제약도 자동으로 충분히 설계됐다고 생각하는 경우
- one-to-many와 many-to-many를 table 구조로 먼저 구분하지 않고 ORM 속성부터 작성하는 경우
- link table에 추가 column이 있는데 단순 연결 table처럼 숨기는 경우
- `cascade`와 DB의 `ON DELETE CASCADE`를 같은 계층의 설정으로 보는 경우
- 관계를 정의했지만 조회 시 eager loading을 고려하지 않아 N+1 query가 생기는 경우

## 관련 문서

- [DatabaseAndORM.md](./DatabaseAndORM.md)
- [FastAPIBackend.md](./FastAPIBackend.md)
