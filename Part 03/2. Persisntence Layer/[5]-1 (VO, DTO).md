## 1. VO 클래스를 생성 (테이블의 설계 기준으로 작성)
- org.zerock.domain 패키지 생성
- BoardVO.java (BoardDTO.java) 인터페이스 생성
- @Data
    - getter/setter, toString()을 위해 사용하는 어노테이션
- private String title;
    - type: Long, String, DATE(java.util.Date), int...
      name: 오라클에 생성한 컬럼명과 일치시켜야 혼돈이 없음
