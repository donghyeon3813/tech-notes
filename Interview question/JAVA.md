### try-with-resource에 대해서 설명해주세요.
객체를 통해 컴퓨터 자원에 직접 접근하는 리소스들은 사용후에 반드시 자원을 해제해야합니다.
일반적인 try-catch문을 사용하였을때는 해당 리소스들을 finally를 통해서 자원을 명시적으로 해제시켜줘야하지만 try-with-resource를 사용하게되면 
AutoCloseable 이 상속되어있는 자원이라면 try 블록이 끝나면서 자동으로 연결을 해제시켜주는 close 메소드를 호출해줍니다.

---

### 자바에서 Checked Exception과 Unchecked Exception에 대해서 설명해주세요.
Checked Exception은 Exception Class와 그의 하위 클래스 중 RuntimeException을 상속받지 않는 예외로 반드시 처리해야하고 
Unchecked Exception은 RuntimeException과 그의 하위클래스들로 런타임 도중 프로그래머의 실수로 인해 발생할 수 있는 예외입니다.
