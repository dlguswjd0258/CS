# :question: ​JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가

### c/c++에서는 컴파일과 실행이 어떻게 동작할까?

-  소프트웨어가 운영체제에 종속적이기 때문에 컴파일 플랫폼과 실행할 플랫폼이 일치해야만 동작했다.

  ex) 컴파일 플랫폼 = Linux, 실행 플랫폼 = window (실행 :x:)

- 위의 문제를 컴파일 할 때 실행할 플랫폼에 맞춰 컴파일하는 방법(크로스 컴파일)으로 해결하곤 했다.



p.s. 플랫폼 = 운영체제 + CPU 아키텍처



### Java에서는 컴파일과 실행이 어떻게 동작할까?

<img src="C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20210718133051393.png" alt="image-20210718133051393" style="zoom:60%;" />

- javac 
  - 자바 개발 키트에 포함된 주요 **자바 컴파일러**
  - 자바 언어 사양을 충족하는 소스 코드를 받아서 자바 가상 머신 사양을 충족하는 바이트코드를 생성한다.

- bytecode
  - 자바 가상 머신이 이해할 수 있는 언어로 변환된 자바 소스 코드
  - 자바 컴파일러에 의해 변환되는 코드의 명령어 크기는 1바이트
  - bytecode의 확장자는 .class
  - 자바 가상 머신만 설치되어 있다면 어떤 운영체제에서라도 실행 가능



### JVM ( Java Virtual Machine )으로 컴파일시 운영체제 신경쓰지 말자

- 소프트웨어로 구성된 에뮬레이터(Emulator:대리 실행기)로서 운영체제와 자바 프로그램 사이에서 자바 프로그램이 실행될 수 있는 환경 제공
- 즉, JVM은 자바 프로그램을 운영체제가 이해할 수 있는 형식으로 변환하여 운영체제에게 전달하는 역할을 수행

- 일반적으로 인터프리터나 JIT 컴파일 방식으로 다른 컴퓨터 위에서 바이트 코드를 실행할 수 있도록 구현
- JVM은 실행할 플랫폼에 의존하기 때문에 자바 프로그램 실행할 운영체제에 맞춰 설치 해야한다.



### JVM 구성요소

<img src="C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20210718131124556.png" alt="image-20210718131124556" style="zoom: 67%;" />

- Class Loader
  - 동적으로 클래스를 로딩해주는 역할
- Runtime Data Areas 
  - 자바 바이트코드를 실행하기 위해 사용하는 메모리 공간
  - `Mehtod Area`
    - JVM이 시작될 때 생성되는 공간으로 클래스 로더가 클래스 파일을 읽어오면 클래스 정보를 파싱해서 저장
    - 어떤 메서드,변수, 정적 변수가 있는지 바이트코드는 어떤지를 저장
  - `Heap`
    - 프로그램을 실행하면서 동적으로 생성된 모든 객체를 저장하는 영역
    - Garbage Collerctor의 대상이 되는 공간
  - `PC(Program Counter) registers`
    - 현재 수행중인 JVM의 명령어 주소를 저장하는 공간
    - 각 스레드는 어떤 메서드를 항상 실행하고 있고 PC는 그 메서드 안에서 몇 번째 줄을 실행하고 있는지 나타내는 역할
  - `stack`
    - 스레드별로 1개만 존재하고, 지역변수나 메서드 매개변수, 임시적으로 사용되는 변수, 메서드의 정보가 저장되는 영역
  - `Native method stacks`
    - 자바 바이트코드가 아닌 다른 언어로 작성된 메서드를 의미
- Execution Engine
  - load된 클래스 파일의 바이트코드를 실행
  - `Interpreter`
    - bytecode를 읽고 해석하는 역할
  - `JIT Compiler(Just-In-Time compiler) `
    - 프로그램이 실행 중인 런타임에 실제 기계어로 변환해 주는 컴파일러
    - 동적 번역이라고도 불리고 프로그램의 실행 속도를 향상시키기 위해 개발
    - 즉, bytecode를 런타임에 바로 기계어로 변환하는데 사용
    - Interpreter의 보조 역할을 하면서 반복되는 코드로 컴파일 된 코드를 저장소에서 꺼내 바로 사용할 수 있도록 한다.
  - `Garbage Collector`
    - 더는 사용하지 않는 메모리를 자동으로 회수
    - 개발자가 따로 메모리 관리 하지 않아도 되므로, 더욱 손쉽게 프로그래밍 할 수 있도록 도움



### JDK와 JRE

![img](https://blog.kakaocdn.net/dn/bdyAwE/btqM4zPa2T4/5qOskY2htyUYDxJBHTDE3K/img.png)

