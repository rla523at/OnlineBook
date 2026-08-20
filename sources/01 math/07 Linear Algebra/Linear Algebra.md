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

이 장은 다음 여덟 묶음으로 학습한다. Matrix representation을 배운 뒤에는 basis를 바꿔 matrix를 단순화하는 흐름과 matrix equation의 해를 분석하는 흐름으로 나뉜다. 두 흐름에서 얻은 개념은 inner product와 orthogonality를 배운 뒤 matrix subspace, least squares와 matrix decomposition에서 다시 합쳐진다.

### Vector Space Structure

- [Vector Space](<01 Vector Space Structure/01 Vector Space.md>)는 addition과 scalar multiplication이 만족해야 하는 공통 규칙을 정의한다.
- [Subspace](<01 Vector Space Structure/02 Subspace.md>)는 vector space 안에서 같은 연산을 유지하는 부분집합을 다룬다.
- [Span](<01 Vector Space Structure/03 Span.md>)은 주어진 vector들로 만들 수 있는 모든 linear combination을 모은다.
- [Linearly Independent](<01 Vector Space Structure/04 Linearly Independent.md>)는 서로 중복되는 direction이 없는지 판별한다.
- [Basis](<01 Vector Space Structure/05 Basis.md>)는 span과 linear independence를 동시에 만족하는 최소한의 표현 체계를 만든다.
- [Coordinate](<01 Vector Space Structure/06 Coordinate.md>)는 ordered basis를 선택해 abstract vector를 scalar column으로 표현한다.

### Linear Maps and Isomorphisms

- [Linear Map](<02 Linear Maps and Isomorphisms/10 Linear Map.md>)은 linear combination을 보존하는 함수를 정의한다.
- [Kernel](<02 Linear Maps and Isomorphisms/11 Kernel.md>)과 [Image](<02 Linear Maps and Isomorphisms/12 Image.md>)은 linear map이 잃는 input direction과 만들 수 있는 output을 설명한다.
- [Vector Space Isomorphism](<02 Linear Maps and Isomorphisms/13 Vector Space Isomorphism.md>)은 두 vector space가 같은 linear structure를 갖는다는 뜻을 정리한다.

### Matrix Representation and Eigenstructure

- [Matrix Representation](<03 Matrix Representation and Eigenstructure/20 Matrix Representation.md>)은 basis를 선택했을 때 linear map이 matrix multiplication으로 나타나는 과정을 유도한다.
- [Change of Basis and Coordinate Matrix](<03 Matrix Representation and Eigenstructure/23 Change of Basis and Coordinate Matrix.md>)은 같은 vector와 linear map의 표현이 basis에 따라 어떻게 바뀌는지 설명한다.
- [Eigenvector & Eigenvalue & EigenSpace](<03 Matrix Representation and Eigenstructure/24 Eigenvector & Eigenvalue & EigenSpace.md>)은 linear map이 direction을 바꾸지 않는 특별한 vector를 찾는다.

### Linear Systems and Rank

- [Linear Systems and Row Reduction](<04 Linear Systems and Rank/21 Linear Systems and Row Reduction.md>)은 matrix equation의 해를 row operation으로 분석한다.
- [Column Space, Row Space, and Rank](<04 Linear Systems and Rank/22 Column Space, Row Space, and Rank.md>)는 해의 존재 조건과 pivot 구조를 column space, row space와 rank로 설명한다.

### Inner Product and Orthogonality

- [Inner Product Space](<05 Inner Product and Orthogonality/30 Inner Product Space.md>)는 vector space에 length와 angle을 다룰 수 있는 구조를 추가한다.
- [Gram Matrix](<05 Inner Product and Orthogonality/31 Gram Matrix.md>)는 basis에서 inner product를 계산하는 matrix를 구성한다.
- [Norm, Distance and Angle](<05 Inner Product and Orthogonality/32 Norm Distance and Angle.md>)은 inner product에서 norm, distance, angle과 orthogonality를 유도한다.
- [Projection and Orthogonal Subset](<05 Inner Product and Orthogonality/33 Projection and Orthogonal Subset.md>)은 orthogonal direction으로 projection coefficient를 분리하는 원리를 설명한다.
- [Gram-Schmidt Process](<05 Inner Product and Orthogonality/34 Gram-Schmidt Process.md>)는 linearly independent subset을 같은 span의 orthogonal subset으로 바꾼다.
- [Orthogonal Complement and Orthogonal Projection](<05 Inner Product and Orthogonality/35 Orthogonal Complement and Orthogonal Projection.md>)은 subspace와 그 orthogonal complement를 이용해 vector를 분해한다.
- [Orthogonal Map](<05 Inner Product and Orthogonality/37 Orthogonal Map.md>)은 inner product와 geometry를 보존하는 linear map을 다룬다.

### Matrix Subspaces and Approximation

- [Four Fundamental Subspaces](<06 Matrix Subspaces and Approximation/36 Four Fundamental Subspaces.md>)는 row space, column space와 두 null space 사이의 orthogonal 관계를 정리한다.
- [Least Squares Problem](<06 Matrix Subspaces and Approximation/38 Least Squares Problem.md>)은 column space로의 projection을 이용해 exact solution이 없는 matrix equation의 가장 가까운 해를 구한다.

### Duality and Adjoint

- [Dual Space](<07 Duality and Adjoint/42 Dual Space.md>)는 vector를 scalar로 측정하는 linear functional과 dual map을 정의한다.
- [Riesz Representation Theorem](<07 Duality and Adjoint/43 Riesz Representation Theorem.md>)은 inner product를 통해 linear functional을 vector로 나타내고 adjoint를 구성한다.

### Matrix Decompositions

- [Schur's Theorem](<08 Matrix Decompositions/39 Schur's Theorem.md>)은 adjoint와 invariant orthogonal complement를 이용해 linear map을 upper triangular matrix로 나타낸다.
- [Symmetric Matrix and Spectral Theorem](<08 Matrix Decompositions/40 Symmetric Matrix and Spectral Theorem.md>)은 symmetry가 Schur form을 diagonal form으로 단순화하는 이유를 설명한다.
- [Singular Value Decomposition](<08 Matrix Decompositions/41 Singular Value Decomposition.md>)은 spectral theorem을 이용해 arbitrary rectangular matrix를 orthogonal direction별 scaling으로 분해한다.

## 읽을 때 확인할 구분

- $v$와 $[v]_\beta$는 같은 것이 아니다. 전자는 vector이고 후자는 basis $\beta$에서 그 vector를 나타내는 coordinate column이다.
- $T$와 $[T]_\beta^\gamma$도 같은 것이 아니다. 전자는 linear map이고 후자는 domain basis $\beta$와 codomain basis $\gamma$에서 그 map을 나타내는 matrix다.
- Equality, isomorphism과 similarity는 서로 다른 관계다. 두 대상이 같은 구조를 표현하더라도 같은 집합이나 같은 matrix일 필요는 없다.
