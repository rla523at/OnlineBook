# Linear Algebra

## 한 줄 요약

Linear algebra는 vector의 linear combination, 이를 보존하는 linear map, 그리고 basis를 선택했을 때 나타나는 coordinate와 matrix를 하나의 구조로 연결한다.

## 무엇을 배우는가

여러 수치가 나열된 column만 vector인 것은 아니다. Polynomial, function, signal과 geometric displacement도 addition과 scalar multiplication을 만족하면 같은 linear-algebraic 방법으로 다룰 수 있다. Linear algebra의 목적은 이러한 대상을 특정 coordinate 표현에 묶어 두지 않고 공통 구조로 이해한 뒤, 계산할 때만 basis를 선택해 matrix로 옮기는 것이다.

이 장에서는 다음 세 층을 구분한다.

1. `Vector`와 `linear map`은 basis를 선택하기 전에도 존재하는 대상이다.
2. `Coordinate column`과 `matrix representation`은 basis를 선택한 뒤 얻는 표현이다.
3. Basis가 바뀌면 표현은 달라지지만 원래 vector와 linear map은 달라지지 않는다.

이 구분을 유지하면 abstract vector space의 정리와 실제 matrix 계산이 어떻게 연결되는지 명확해진다.

## 학습 순서

### Vector space의 구조

- [Vector Space](<01 Vector Space.md>)는 addition과 scalar multiplication이 만족해야 하는 공통 규칙을 정의한다.
- [Subspace](<02 Subspace.md>)는 vector space 안에서 같은 연산을 유지하는 부분집합을 다룬다.
- [Span](<03 Span.md>)은 주어진 vector들로 만들 수 있는 모든 linear combination을 모은다.
- [Linearly Independent](<04 Linearly Independet.md>)는 서로 중복되는 direction이 없는지 판별한다.
- [Basis](<05 Basis.md>)는 span과 linear independence를 동시에 만족하는 최소한의 표현 체계를 만든다.
- [Coordinate](<06 Coordinate.md>)는 ordered basis를 선택해 abstract vector를 scalar column으로 표현한다.

### Linear map과 matrix

- [Linear Map](<10 Linear Map.md>)은 linear combination을 보존하는 함수를 정의한다.
- [Kernel](<11 Kernel.md>)과 [Image](<12 Image.md>)은 linear map이 잃는 input direction과 만들 수 있는 output을 설명한다.
- [Vector Space Isomorphism](<13 Vector Space Isomorphism.md>)은 두 vector space가 같은 linear structure를 갖는다는 뜻을 정리한다.
- [Matrix Representation](<20 Matrix Representation.md>)은 basis를 선택했을 때 linear map이 matrix multiplication으로 나타나는 과정을 유도한다.
- [Linear Systems and Row Reduction](<21 Linear Systems and Row Reduction.md>)과 [Column Space, Row Space, and Rank](<22 Column Space, Row Space, and Rank.md>)은 matrix equation의 해와 rank를 연결한다.
- [Change of Basis and Coordinate Matrix](<23 Change of Basis and Coordinate Matrix.md>)은 같은 vector와 linear map의 표현이 basis에 따라 어떻게 바뀌는지 설명한다.
- [Eigenvector & Eigenvalue & EigenSpace](<24 Eigenvector & Eigenvalue & EigenSpace.md>)은 linear map이 direction을 바꾸지 않는 특별한 vector를 찾는다.

### Inner product와 geometric decomposition

- [Inner Product Space](<30 Inner Product Space.md>)부터 [Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)까지는 length, angle, orthogonality와 orthogonal basis를 만든다.
- [Orthogonal Complement and Orthogonal Projection](<35 Orthogonal Complement and Orthogonal Projection.md>)부터 [Least Squares Problem](<38 Least Squares Problem.md>)까지는 vector를 orthogonal component로 분해하고 가장 가까운 해를 구한다.
- [Schur's Theorem](<39 Schur's Theorem.md>), [Symmetric Matrix and Spectral Theorem](<40 Symmetric Matrix and Spectral Theorem.md>)과 [Singular Value Decomposition](<41 Singular Value Decomposition.md>)은 적절한 orthonormal basis를 선택해 matrix의 구조를 단순하게 만든다.
- [Dual Space](<42 Dual Space.md>)와 [Riesz Representation Theorem](<43 Riesz Representation Theorem.md>)은 vector를 scalar로 측정하는 linear functional과 inner product의 관계를 설명한다.

## 읽을 때 확인할 구분

- $v$와 $[v]_\beta$는 같은 것이 아니다. 전자는 vector이고 후자는 basis $\beta$에서 그 vector를 나타내는 coordinate column이다.
- $T$와 $[T]_\beta^\gamma$도 같은 것이 아니다. 전자는 linear map이고 후자는 domain basis $\beta$와 codomain basis $\gamma$에서 그 map을 나타내는 matrix다.
- Equality, isomorphism과 similarity는 서로 다른 관계다. 두 대상이 같은 구조를 표현하더라도 같은 집합이나 같은 matrix일 필요는 없다.
