###[1. 폴딩](#1)
###[2. 참조 링크 삽입](#2)
###[3. 함축적 링크 사용](#3)
---
### 1
  ~~~
  div markdown=”1” 은 jekyll에서 html사이에 markdown을 인식 하기 위한 코드
    <details>
    <summary>접기/펼치기 버튼</summary>
    <div markdown="1">

    ```java
    public void test(){

    }
    ```

    </div>
    </details>
  ~~~
---
### 2
  ~~~
  링크 삽입 시 문법: \[주소에 대한 설명] \[참조 번호]  
  참조 번호 작성 문법: \[참조 번호]: 주소  

  이 부분은 [Google] [1]을 참조하시면 됩니다.  

  이 부분도 [Google] [1]을 참조하시고 저 부분은 [Facebook] [2]을 참조하세요.  

  [1]: http://www.google.com  
  [2]: http://www.facebook.com
  ~~~
---
### 3
  ~~~
  링크 삽입 시 문법 : [참조 이름]
  참조 이름 작성 문법 : [참조 이름]: 주소

  이 부분은 [Google]를 참조하시면 됩니다.  

  이 부분도 [Google]를 참조하시고 저 부분은 [Facebook]을 참조하세요.  

  [Google]: http://www.google.com  
  [Facebook]: http://www.facebook.com  

  *사실 참조 링크, 함축적 링크에서 참조 번호, 이름은 글 어디든 적어두셔도 괜찮습니다.*
  *URL 주소만 적을 때는 <주소> 형태로 작성하시면 됩니다.*
  ~~~
---
