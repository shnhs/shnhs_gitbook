# MockMultipart

## 파일 핸들링 테스트코드

파일을 다루는 컨트롤러 혹은 메서드도 테스트코드를 작성할 수 있다.

파일 관련 Mocking을 지원하기 위해 `MockMultipartFile` 이라는 클래스를 제공한다.

```java
MockMultipartFile(String name, String originalFilename, String contentType, InputStream contentStream)

MockMultipartFile file = new MockMultipartFile("image",
                                                "test.png",
                                                "image/png", // 테스트에 사용하는 파일의 타입 지정
                                                new FileInputStream("업로드 할 실제 파일 path 입력"));
```

컨트롤러 동작을 Mocking할 때 MockMvc의 `perform()` 메서드를 사용한다.\
multipart/form-data 형식의 API라면 `perform().post()` 가 아닌\
별도로 지원하는 `perform().multipart("/{URI}")`를 사용한다.



### 강의 간략 후기

회사에서 잠시 테스트 코드를 작성하기를 '시도'했을 때 압축파일 관련 테스트 할 때\
MockMultipart 파일 가지고 꽤 힘겹게 이것저것 해보았던 기억이 있다.\
지금은 이전에 강의를 통해 기반을 그나마 닦아 놓았던 덕분인지!\
아니면 지금 회사의 코드가 설계가 엉망인건지, 당시에 힘들었던 기억이 무색하게\
MockMultipartFile을 다루어 테스트하는게 꽤나 쉽게 느껴졌다.
