# Mark Down

## code block inside lists
리스트 안에 있는 코드 블럭을 만들고 싶으면 들여쓰기를 하면 된다.

```
* list
  \```
  code block in list
  \```
  * in nested list  
    \```
    code block in nested list 
    \```
```

* list
  ```
  code block in list
  ```
  * in nested list  
    ```
    code block in nested list 
    ```

> Reference   
> [gist.github](https://gist.github.com/clintel/1155906)  

## Collapsible Text

Collapsible Text 를 사용하고 싶으면 다음과 같이 하면 된다.

```
<details> <summary> 제목 </summary>
내용
</details>
```

<details> <summary> 제목 </summary>
내용
</details>

</br></br>

만약 제목을 Heading 형태로 만들기 위해서는 추가 태그를 붙이면 된다.

```
<details> <summary> <h3 style="display:inline-block"> 제목 </h3></summary>
내용
</details>
```

<details> <summary> <h3 style="display:inline-block"> 제목 </h3></summary>
내용
</details>

</br></br>

만약 말머리표를 사용하고 싶다면 한줄에 모든내용을 다 적어야 한다.

하지만 이는 상당히 불편함으로 왠만하면 사용하지 않는게 좋다.

```
* <details> <summary> 제목 </h3></summary> 내용 </details>
```

* <details> <summary> 제목 </h3></summary> 내용 </details>




