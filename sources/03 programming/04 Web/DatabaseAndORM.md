# Database And ORM

## 한 줄 요약

PostgreSQL은 데이터를 저장하는 DBMS이고, SQLAlchemy는 Python 코드에서 DB 테이블과 row를 객체처럼 다루게 해 주는 ORM이며, Alembic은 DB schema 변경 이력을 관리하는 migration 도구다.

## PostgreSQL

PostgreSQL은 관계형 데이터베이스다. 사용자, 게시글, 주문, 파일 메타데이터처럼 request가 끝난 뒤에도 남아야 하는 데이터를 저장한다.

애플리케이션은 직접 파일에 데이터를 쓰기보다 DB connection을 통해 PostgreSQL에 query를 보낸다.

```text
FastAPI code
  -> SQLAlchemy session
  -> PostgreSQL connection
  -> table row read/write
```

## DATABASE_URL과 환경변수

Python 애플리케이션은 보통 `DATABASE_URL` 같은 연결 문자열로 DB에 접속한다.

```text
postgresql+psycopg2://<user>:<password>@<host>:<port>/<db>
```

운영에서는 이 값을 코드에 직접 쓰지 않고 환경변수나 `.env` 파일에서 읽는다.

예:

```text
POSTGRES_USER=myapp
POSTGRES_PASSWORD=<password>
POSTGRES_HOST=127.0.0.1
POSTGRES_PORT=5432
POSTGRES_DB=myapp
```

이렇게 나눠 두면 dev, test, staging, production이 서로 다른 DB를 사용하도록 쉽게 분리할 수 있다.

## SQLAlchemy 모델

SQLAlchemy 모델은 DB 테이블 구조를 Python 클래스로 표현한다.

```python
class Article(DbBaseModel):
    __tablename__ = 'articles'

    id = Column(Integer, primary_key=True)
    title = Column(String, nullable=False)
```

SQLAlchemy 모델은 API 입출력 schema가 아니다. API의 request/response 구조는 Pydantic 모델로 따로 정의하는 편이 안전하다.

## Session

SQLAlchemy session은 DB 작업을 수행하기 위한 작업 단위다.

```python
article = Article(title='Hello')
db.add(article)
db.flush()
```

FastAPI에서는 보통 HTTP request 1회 동안 session 하나를 만들고, request가 끝나면 닫는다. session의 자세한 lifecycle은 [FastAPIBackend.md](./FastAPIBackend.md)를 참고한다.

## Alembic migration

Alembic은 DB schema 변경 이력을 migration 파일로 관리한다.

예를 들어 새 테이블을 추가하거나 컬럼을 변경하면 migration revision을 만든다.

```powershell
python -m scripts.db_manager --make-migration -m "add project tables"
python -m scripts.db_manager --migrate
```

migration을 관리하는 이유는 다음과 같다.

- DB schema 변경 이력을 코드로 남길 수 있다.
- 여러 환경의 DB schema를 같은 순서로 맞출 수 있다.
- 배포 시 어떤 schema 변경이 적용됐는지 추적할 수 있다.

autogenerate 결과는 그대로 믿지 말고 검토해야 한다. rename, constraint 변경, nullable 변경, data migration은 자동 생성 결과가 의도와 다를 수 있다.

## ForeignKey와 relationship

`ForeignKey`는 DB 테이블 사이의 참조 제약이고, `relationship()`은 Python 객체 사이의 탐색 경로다.

```python
class Comment(DbBaseModel):
    article_id = Column(Integer, ForeignKey('articles.id', ondelete='CASCADE'), nullable=False)
    article = relationship('Article', back_populates='comments')
```

- `article_id`
  - DB에 실제로 저장되는 컬럼
- `ForeignKey('articles.id')`
  - `comments.article_id`가 `articles.id`를 참조한다는 DB 제약
- `article`
  - Python 코드에서 `comment.article`처럼 관련 객체로 이동하기 위한 ORM 속성

## one-to-many

하나의 게시글에 여러 댓글이 달리는 관계는 one-to-many다.

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

코드에서는 이렇게 사용할 수 있다.

```python
article.comments
comment.article
```

## many-to-many와 link table

학생과 강의는 many-to-many 관계로 표현하기 쉽다.

- 한 학생은 여러 강의를 수강할 수 있다.
- 하나의 강의에는 여러 학생이 등록될 수 있다.

이 관계는 중간 테이블로 표현한다.

```python
class StudentCourseEnrollment(DbBaseModel):
    __tablename__ = 'student_course_enrollments'

    student_id = Column(Integer, ForeignKey('students.id', ondelete='CASCADE'), primary_key=True)
    course_id = Column(Integer, ForeignKey('courses.id', ondelete='CASCADE'), primary_key=True)
```

SQLAlchemy에서는 순수 연결 테이블을 내부 구현처럼 감추고 객체에서는 목록처럼 다룰 수 있다.

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

단, link table에 `enrolled_at`, `status`, `grade` 같은 추가 컬럼이 있다면 단순 연결 테이블이 아니라 별도 도메인 객체로 다루는 편이 더 명확할 수 있다.

## back_populates

`back_populates`는 관계의 양쪽 끝을 서로 연결한다.

```python
Article.comments <-> Comment.article
Student.courses <-> Course.students
```

양쪽에 `back_populates`를 맞춰 두면 ORM이 양방향 관계를 일관되게 관리할 수 있다.

## cascade

`cascade='all, delete-orphan'`은 부모 객체와 강하게 묶인 하위 객체에 자주 사용한다.

```python
class Article(DbBaseModel):
    comments = relationship(
        'Comment',
        back_populates='article',
        cascade='all, delete-orphan',
    )
```

의미는 다음과 같다.

- 부모인 `Article`이 삭제될 때 관련 `Comment`도 함께 정리된다.
- 어떤 `Comment`가 더 이상 어떤 `Article`에도 속하지 않으면 고아 row로 남기지 않는다.

## eager loading과 selectinload

`relationship()`은 관계 자체를 정의한다. 관련 객체를 언제 읽을지는 별도 문제다.

```python
select(Article).options(
    selectinload(Article.comments),
)
```

이 코드는 게시글 목록을 가져올 때 각 게시글의 댓글도 별도 최적화된 query로 미리 읽어 두라는 의미다.

관계를 정의하는 것이 `relationship()`이라면, 조회 시점에 로딩 방식을 정하는 것이 `selectinload()`다.

## 자주 하는 실수

- SQLAlchemy 모델과 Pydantic schema를 같은 것으로 보는 경우
- `ForeignKey`만 있으면 Python 객체 관계가 자동으로 생긴다고 생각하는 경우
- migration autogenerate 결과를 검토 없이 적용하는 경우
- local reset과 production migration을 같은 수준의 작업으로 생각하는 경우
- 관계를 정의했지만 조회 시 eager loading을 고려하지 않아 N+1 query가 생기는 경우

## 관련 문서

- [FastAPIBackend.md](./FastAPIBackend.md)
- [WebDeployment.md](./WebDeployment.md)
