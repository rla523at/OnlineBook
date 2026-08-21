# sources 학습 문서 작업 규칙

## 기준 독자

- 문서별 기준 독자는 해당 문서 주제를 처음 학습하는 독자로 잡는다.

## 스킬 적용

- 학습 문서를 새로 만들거나 내용 또는 문서 내부 구조를 실질적으로 수정할 때는 `$learning-document-writer`를 사용한다. writer는 작성 후 `$learning-document-reviewer`를 통한 읽기 전용 검토까지 수행한다.
- 학습 문서를 변경하지 않고 검토만 할 때는 `$learning-document-reviewer`를 사용한다.
- 학습 문서나 폴더를 추가·삭제·이동하거나 문서 순서·묶음·폴더 구성을 변경하기 전에는 `$learning-content-structure-review`를 사용한다.

## Jupyter Book 검증

- `sources` 하위 문서를 수정한 뒤에는 페이지가 의도대로 반영되고 Jupyter Book build가 정상 완료되는지 확인한다.
- 기본 검증 command는 `python jb_manager.py --build`다. 이 command는 `toc.yml`을 재생성한 뒤 `jupyter_book build --html`을 실행하고 종료된다.
- 문서 파일을 추가, 삭제, 이동, rename한 경우에는 `toc.yml` 구조가 바뀌므로 `python jb_manager.py --build`를 다시 실행해서 TOC와 build 결과를 확인한다.
- 작업을 마칠 때는 `python jb_manager.py --build`를 실행했는지, 또는 이미 실행 중인 preview에서 확인했는지를 명시한다.
