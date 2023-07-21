### ***JVM 이란?***
JVM(Java Virtual Machine)은 OS 위에서 동작하며 자바나 코틀린으로 개발된 애플리케이션을 실행할 수 있도록하는 자바 가상머신입니다. C, C++ 등 다른 언어들은 각 운영체제에 맞게 컴파일이되고 실행되지만 자바는 JVM의 존재로 운영체제와 상관없이 바이트 코드로 컴파일되며 이 바이트 코드는 어떤 운영체제이든지 동일하게 동작합니다. 즉, JVM 덕분에 **자바는 운영체제에 종속적이지 않다**는 장점이 있습니다. 또한, JVM에 탑재되어 있는 **GC(Garbage Collector)가 메모리 누수 등 메모리를 관리**해주기 때문에 개발자가 메모리를 관리하지 않아도 됩니다.
> 운영체제에 종석적이지 않은 것은 자바이며 JVM은 각 운영체제, 플랫폼에 맞는 것을 설치해야 합니다.

하지만 자바는 다른 언어보다 애플리케이션이 실행되기까지 JVM이라는 과정을 한번 더 거치고 기본적으로 바이트 코드를 인터프리팅하는 방식으로 명령을 실행하기 때문에 **속도가 느리다는 단점**이 존재합니다.

<p align="center">
  <img width="575" alt="JVM 구조" src="https://github.com/eello/solve-algorithm/assets/33685064/80a45a9e-e643-47df-a56a-d3094788c39a"/>
  <br>
  출처: <a href="https://coding-factory.tistory.com/828">코딩팩토리</a>
</p>


JVM은 다음과 같이 크게 3가지로 구성되어 있습니다.
- Class Loader
- Execution Engine
- Runtime Data Area

---

### ***Class Loader***
JVM은 자바 실행시 필요한 클래스들만 가져와서 JVM에 연결하는 동적 클래스 로딩 방식을 사용합니다. **Class Loader는 애플리케이션에서 필요할때마다 JVM 내로 클래스 파일(\*.class)을 동적으로 로드하고 링크를 통해 배치하는 작업을 수행하는 모듈**입니다. 즉, **로드된 바이트 코드(\*.class)를 JVM 메모리 영역인 Runtime Data Area에 배치하는 역할을 수행**합니다. 클래스 파일의 로딩 순서는 다음과 같습니다.

<p align="center">
  <img width="430" alt="클래스 파일 로딩 순서" src="https://github.com/eello/solve-algorithm/assets/33685064/afe4e95f-7eeb-4372-835f-5523dfeab80a">
  <br>
  출처: <a href="https://inpa.tistory.com/entry/JAVA-%E2%98%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD-%EC%8B%AC%ED%99%94%ED%8E%B8">inpa.tistory.com</a>
</p>

1. `Loading` - 클래스 파일을 가져와서 JVM의 메모리에 로드합니다.
2. `Linking` - 클래스 파일을 사용하기 위해 검증하는 과정입니다.
    2-1. Verifying - 읽어들인 클래스가 JVM 명세에 명시된 대로 구성되어 있는지 검사합니다.
    2-2. Preparing - 클래스가 필요로하는 메모리를 할당합니다.
    2-3. Resolving - 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경합니다.
3. `Initialization` - 클래스 변수들을 적절한 값으로 초기화 합니다.

> ***바이트 코드 vs 바이너리 코드***
> **바이트 코드**는 가상머신(VM)에서 돌아가는 실행 프로그램을 위한 이진 표현법입니다. 따라서 자바 바이트 코드(Java bytecode)는 JVM이 이해할 수 있는 언어로 변환된 자바 소스코드를 의미합니다. 자바 컴파일러에 의해 변환되며 코드의 명령어 크기가 1바이트라서 자바 바이트 코드라 불립니다.
>
> **바이너리 코드**는 이진 코드라고도 하며 컴퓨터가 인식할 수 있는 0과 1로 구성된 이진코드입니다.
>
> 즉, **CPU가 이해하는 언어는 바이너리 코드이며 가상머신이 이해하는 코드는 바이트 코드**입니다.

`Loading` 과정에서 클래스를 동적으로 로딩하는 방식에는 2가지가 존재합니다.
- 로드타임 동적 로딩(load-time dynamic loading)
- 런타임 동적 로딩(run-time dynamic loading)

#### 로드타임 동적 로딩(load-time dynamic loading)
하나의 클래스를 로딩하는 과정에서 필요한 클래스를 동적으로 로딩하는 것이 로드타임 동적 로딩입니다. 예제 코드입니다.
```java
public class HelloWorld {
   public static void main(String[] args) {
      System.out.println("로드타임 동적 로딩!!");
   }
}
```

위 코드를 실행했을 때 일어나는 과정은 다음과 같습니다.
1. JVM이 시작되고 부트스트랩 클래스 로더가 생성됩니다.
2. 모든 클래스가 상속받고 있는 Object 클래스를 로딩합니다.
3. 클래스 로더가 `HelloWorld` 클래스를 로딩하기 위해 `HelloWorld.class` 파일을 읽습니다.
4. 이 때, 실행에 필요한 `java.lang.String`, `java.lang.System` 클래스를 로딩합니다.
5. `HelloWorld` 클래스를 로딩합니다.

이렇게 **`HelloWorld` 클래스를 로딩하기 위해 `HelloWorld.class` 파일을 읽고 필요한 클래스들을 로딩하는 것이 로드타임 동적 로딩**입니다.

#### 런타임 동적 로딩(run-time dynamic loading)
Class Loader는 기본적으로 로드타임 동적 로딩 과정을 실행합니다. 애플리케이션이 실행되기 전까지 어떤 클래스를 참조하는지 알 수 없는 경우 실행시 로딩하는 런타임 동적 로딩을 사용합니다.

```java
public class RuntimeLoading {
	public static void main(String[] args) {
		try {
			Class clazz = Class.forName("java.lang.String");
			Object obj = clazz.newInstance();
		} catch(Exception ex) {
			ex.printStackTrace();
		}
	}
}
```

위 코드를 실행했을 때 기본적으로 로드타임 동적 로딩의 과정을 거치게됩니다. 하지만 `Class.forName("java.lang.String");` 메소드와 같이 실행하기 전까지 어떤 클래스를 참조하는지 모르는 경우가 발생합니다. 따라서, 로드타임 동적 로딩 과정까지는 아무 클래스도 읽지 않고(위 예제에서는 실행하기 위해 필요한 클래스가 없기때문) `Class.forName("java.lang.String");` 메소드가 실행되는 순간에 `java.lang.String` 클래스를 읽어옵니다. 이처럼 클래스를 로딩할 때가 아닌 **코드를 실행하는 순간에 필요한 클래스를 로딩하는 것이 런타임 동적 로딩**입니다.

---

### ***Execution Engine***
Class Loader는 애플리케이션이 실행될 때 필요한 클래스의 바이트 코드를 JVM의 Runtime Data Area에 적재합니다. **Execution Engine(실행 엔진)은 Runtime Data Area에 적재된 바이트 코드를 명령어 단위로 읽어 실행하는 역할을 수행**합니다. 하지만 바이트 코드는 CPU가 아닌 가상머신이 이해하는 코드입니다. 결국 애플리케이션이 실행되려면 CPU가 이해하는 기계어로 변환해야하는데 그 역할을 하는 **`인터프리터`** 방식과 **`JIT 컴파일러`** 두 가지 방식이 존재하며 JVM은 이 두 방식을 혼합하여 사용합니다.

<p align="center">
  <img width="552" alt="실행 엔진" src="https://github.com/eello/solve-algorithm/assets/33685064/5354463c-9295-4527-890d-6acc97cad254">
  <br>
  출처: <a href="https://inpa.tistory.com/entry/JAVA-%E2%98%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD-%EC%8B%AC%ED%99%94%ED%8E%B8">inpa.tistory.com</a>
</p>

#### ***Interpreter***
**인터프리터 방식은 바이트 코드 명령어를 하나씩 읽고 해석하여 바로 실행합니다. JVM에서 바이트 코드를 실행할때 기본이되는 방식입니다.** 하지만 여러번 호출되는 메소드가 존재한다면 매번 해석하고 실행해야하기 때문에 효율이 떨어집니다.

#### ***JIT Compiler(Just-In-Time Compiler)***
JIT 컴파일러 방식은 인터프리터 방식의 단점을 보완하기 위해 도입된 방식입니다. **일정 기준 이상 반복된 코드를 발견하여 바이트 코드 전체를 Native Code로 컴파일하고 캐싱하였다고 해당 코드를 실행할 때 인터프리팅하지 않고 Native Code를 직접 실행하는 방식입니다.** 컴파일된 Native Code를 직접 실행하는 것이기 때문에 매번 인터프리팅하는 것보다 전체적으로 빠릅니다.
<br>

> ***그렇다면 JIT 컴파일러 방식이 무조건 좋은가?***
>
> 바이트 코드를 Native Code로 컴파일하는 것에도 비용이 소모됩니다. 따라서 **JVM은 인터프리터 방식과 JIT 컴파일러 방식을 혼합하여 사용합니다. 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고 기본적으로 인터프리터 방식을 사용하다가 일정 기준이 넘어가면 JIT 컴파일러 방식으로 실행하게 됩니다.**
> <br>

---

### ***Runtime Data Area***

<p align="center">
  <img width="815" alt="Runtime Data Area" src="https://github.com/eello/solve-algorithm/assets/33685064/864f29e9-52cc-4d21-b819-398dbaf00a99">
  <br>
  출처: <a href="https://coding-factory.tistory.com/828">코딩 팩토리</a>
</p>

런타임 데이터 영역은 **애플리케이션을 실행할 때 사용하는 데이터를 적재하는 영역입니다.** 모든 쓰레드가 공유하는 `Method Area`와 `Heap Area`가 있으며 각 쓰레드마다 할당되는 `Stack Area`, `PC Register`, `Native Method Stack`이 존재합니다.

- **모든 쓰레드 공유**
    - `Method Area`
    - `Heap Area`
- **각 쓰레드마다 할당**
    - `Stack Area`
    - `PC Register`
    - `Native Method Stack`

#### ***Method Area(메소드 영역)***
메소드 영역은 JVM 시작시 생성되며 종료시까지 유지됩니다. 클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보와 같은 각종 필드 정보를 포함합니다. 또한, 메소드 정보, 데이터 타입 정보, Constant Pool, static 변수, final class 등이 생성되는 영역입니다.

> ***Constant Pool***
> 메소드 영역에 존재하는 별도의 관리 영역으로 **각 클래스 또는 인터페이스 마다 별도의 Constant Pool 테이블이 존재하며 클래스를 생성할 때 참조해야할 정보들을 상수로 가지고 있는 영역**이며 **상수 자료형을 저장하여 참조하고 중복을 막는 역할을 수행**합니다. JVM은 Constant Pool을 통해 메소드나 필드의 실제 메모리 상의 주소를 찾아 참조합니다.

#### ***Heap Area(힙 영역)***
힙 영역은 메소드 영역과 마찬가지로 모든 쓰레드가 공유합니다. 프로그램 상에서 데이터를 저장하기 위해 런타임 시 동적으로 할당하여 사용하는 영역으로 **new 키워드로 생성된 객체와 배열 등 Reference Type이 저장되는 영역입니다.**

힙 영역에 생성된 객체와 배열은 JVM의 스택 영역이나 다른 객체의 필드에서 참조됩니다. 즉, 힙의 참조 주소는 스택 영역에서 저장하고 있으며 해당 객체를 통해서만 힙 영역에 있는 인스턴스를 핸들링할 수 있습니다.

만약, 힙 영역에 있는 객체나 배열은 스택 영역에서 참조되지 않고 있다면 의미 없는 객체이기 때문에 GC(Garbage Collector)가 주기적으로 해당 객체를 힙 영역에서 메모리를 해제합니다.

#### ***Stack Area(스택 영역)***
스택 영역은 각 쓰레드마다 할당되는 영역입니다. 쓰레드가 시작될 때 할당되며 쓰레드가 종료되면 해당 스택도 해제됩니다. 스택 영역에는 힙 영역과 달리 Primitive 자료형(int, long, boolean 등)과 임시적으로 사용되는 변수나 정보들이 저장되는 영역입니다. 메소드 호출 시 `스택 프레임`이 생성되고 메소드 호출이 끝나면 프레임별로 삭제됩니다.

<p align="center">
  <img width="508" alt="스택 영역" src="https://github.com/eello/solve-algorithm/assets/33685064/a247709e-afbf-4255-89ad-387d3040f514">
  <br>
  출처: <a href="https://inpa.tistory.com/entry/JAVA-%E2%98%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD-%EC%8B%AC%ED%99%94%ED%8E%B8">inpa.tistory.com</a>
</p>

> ***스택 프레임***
> 
> **스택 프레임**은 메소드가 호출될 때마다 만들어지며, **현재 실행중인 메소드 상태를 저장하는 곳으로 메소드의 매개변수, 지역변수, 리턴값, 연산시 결과값 등이 있습니다.** 메소드 호출 범위가 종료되면 스택에서 제거됩니다.

new 연산을 이용해서 생성한 객체는 힙 영역에 저장됩니다. 하지만 힙 영역에 있는 객체를 핸들링하기 위해서는 해당 객체를 참조하고 있는 변수가 필요합니다. 이 때, 이 변수는 스택 영역에 저장됩니다. 예를들어 `Person p = new Person();`으로 객체를 생성하였을 때 Person 객체는 힙 영역에 저장되고 이를 참조하는 변수 `p`는 스택 영역에 저장됩니다.

프로세스가 메모리에 로드될 때, 즉 JVM이 실행될 때 운영체제로부터 메모리를 할당 받을 때 런타임 시에 스택 사이즈가 고정되어 있어, 런타임시 스택 사이즈를 바꿀 수 없습니다. 만약, 프로그램 실행 중 스택 메모리가 충분하지 않다면 `StackOverFlowError`가 발생합니다.

#### ***PC Register(PC 레지스터)***
스택 영역과 마찬가지로 쓰레드가 시작될 때 할당되는 영역으로, **현재 수행중인 JVM 명령어 주소를 저장하는 공간**입니다. CPU에 직접 연산을 수행하도록 하는 것이 아닌, **현재 작업하는 내용을 CPU에게 연산으로 제공해야하며, 이를 위한 버퍼 공간**입니다.
> JVM 명령의 주소는 쓰레드가 어떤 부분을 무슨 명령으로 실행해야할 지에 대한 기록을 가지고 있습니다.

#### ***Native Method Stack(네이티브 메소드 스택)***

<p align="center">
  <img width="695" alt="네이티브 메소드 스택" src="https://github.com/eello/solve-algorithm/assets/33685064/d9fb80dd-4333-4acc-a758-aede94ca6163">

  <br>
  출처: <a href="https://inpa.tistory.com/entry/JAVA-%E2%98%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD-%EC%8B%AC%ED%99%94%ED%8E%B8">inpa.tistory.com</a>
</p>

네이티브 메소드 스택 영역은 **바이트 코드가 아닌 실제 실행할 수 있는 JIT 컴파일러로 컴파일된 네이티브 코드와 자바 이외의 C, C++, 어셈블리 등으로 작성된 네이티브 코드를 실행시키는 영역**입니다. 일반적으로 메소드를 실행하는 경우 JVM의 스택 영역에 스택 프레임으로 쌓이다가 해당 메소드 내부에 네이티브 방식을 사용하는 메소드가 있다면 해당 메소드는 네이티브 메소드 스택에 쌓입니다.

> 네이티브 방식을 사용하는 메소드
> 
> 네이티브 방식을 사용하는 메소드는 C, C++ 등으로 구현되어있는 라이브러리를 사용하는 경우로 JNI(Java Native Interface)를 통해 Native Method Library를 사용합니다.

---

### ***Garbage Collection***
```java
Person person = new Person();
person.setName("e");
person = null; // garbage 발생

person = new Person();
person.setName("eello");
```

위 코드에서 보면 처음에 person은 이름이 e인 Person 객체를 참조하고 있습니. 하지만 person을 null로 초기화 하고 새로운 이름이 eello인 새로운 Person 객체로 할당하면서 이름이 e인 Person 객체는 누구도 참조하지 않는 가비지가 되었습니다.

하지만 개발자는 이런 상황에서 따로 이름이 e인 Person 객체에 대해 메모리 관리를 하지 않습니다. 그 이유는 ***JVM에 탑재되어 있는 가비지 컬렉터(GC: Garbage Collector)가 메모리를 관리해주기 때문입니다. GC는 메모리 누수를 방지하기 위해 주기적으로 검사하여 정리해줍니다.***

> `System.gc()` 메소드를 호출해 직접 GC를 호출할 수 있지만, 시스템 성능에 큰 영향을 미치게 됩니다.

Runtime Data Area에서 알아본 JVM의 Heap 영역은 2가지 전제로 설계되었습니다.
- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)이 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

즉, 객체는 대부분 일회성이며 메모리에 오래 남아있는 경우는 드물다는 것입니다. 따라서 객체의 생존 기간에 따라 Heap 영역을 **`Young`** 과 **`Old`** 두 영역으로 나누어 설계되었습니다.

<p align="center">
  <img width="307" alt="image" src="https://github.com/eello/solve-algorithm/assets/33685064/fab4406e-f462-4823-84c3-d795900b8c06">

  <br>
  출처: <a href="https://mangkyu.tistory.com/118">망나니개발자</a>
</p>

> 초기에는 Perm 영역이 존재했지만 Java 8부터 제거되었습니다.

#### ***Young 영역***
***Young 영역은 새롭게 생성된 객체가 할당되는 영역*** 입니다. 대부분의 객체가 금방 접근 불가능한 상태가되기 때문에 많은 객체가 Young 영역에 생성되었다가 사라집니다. 이 Young 영역에 대한 가비지 컬렉션을 `Minor GC`라고 합니다.

#### ***Old 영역***
Old 영역의 객체들은 ***Minor GC에서 오랫동안 살아남은 객체들이 Promotion하는 영역*** 입니다. 즉, ***오랫동안 사용되는 객체가 Old 영역으로 복사*** 되어 집니다. Young 영역보다 크게 할당되며 Young 영역에 존재하는 객체보다 오랫동안 Reachable(접근 가능한 상태)이 유지되기 때문에 가비지는 적게 발생합니다. 이 Old 영역에 대한 가비지 컬렉션을 `Major GC`라고 합니다.

> ***Young 영역보다 크게 할당되는 이유?***
>
> Young 영역의 수명이 짧은 객체들은 큰 공간을 필요로 하지 않으며 큰 객체들은 Young 영역이 아니라 바로 Old 영역에 할당되기 때문입니다.

2가지 전제 중에 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다 했습니다. 즉, 예외적인 상황으로 Old 영역의 객체가 Young 영역의 객체를 참조하는 경우가 존재할 수도 있습니다. 객체는 스택 영역에서 해당 객체에 참조가 이루어지지 않으면 GC의 대상이됩니다. 하지만 이러한 예외 상황에서 GC의 대상이 되었지만 Old 영역에서 참조가 되어 메모리 해제가 되지말아야 하는 경우도 존재합니다. 그러면 GC할 때 이러한 예외상황을 고려하여 Old 영역에서 참조하는 Young 영역의 객체를 식별하고 GC의 대상에서 제외시켜야 합니다. 이러한 비효율적인 작업을 해결하기 위해 Old 영역에는 512byte의 덩어리로 되어있는 **`카드 테이블(Card Table)`** 이 존재합니다. 이 카드 테이블에는 Old 영역의 객체가 Young 영역의 객체를 참조할 때마다 그 정보가 표시되게 됩니다. 따라서 **Minor GC가 실행될 때 카드 테이블만 조회하여 GC의 대상인지 식별할 수 있게 됩니다.**

<p align="center">
  <img width="394" alt="image" src="https://github.com/eello/solve-algorithm/assets/33685064/9fa8d56d-877f-4862-98b5-02fdce43e0d4">

  <br>
  출처: <a href="https://mangkyu.tistory.com/118">망나니개발자</a>
</p>

### ***Garbage Collection의 동작방식***
Young 영역과 Old 영역 서로 다른 메모리 구조를 가지고 있기 때문에 세부적으로는 동작방식이 다르지만 기본적으로 다음 2가지의 공통 단계를 따릅니다.
1. Stop The World
2. Mark and Sweep

##### ***1. Stop The World***
GC가 실행될 때는 GC를 실행하는 쓰레드 이외의 모든 쓰레드의 작업이 중단되고, GC가 완료되면 작업이 재개됩니다. 이름 그대로 **Stop The World 과정은 GC 실행을 위해 JVM이 애플리케이션의 실행을 멈추는 작업**입니다.

> GC 성능 개선을 위해 튜닝을 한다고 하면 보통 stop-the-world의 시간을 줄이는 작업을 하는 것입니다.

##### ***2. Mark and Sweep***
Stop The World 과정을 통해 애플리케이션의 실행이 멈추면 GC는 스택의 모든 변수 또는 접근가능한 객체를 스캔하면서 어떤 객체를 참고하고 있는지 탐색을 하면서 사용되고 있는 **메모리를 식별**합니다. 이렇게 식별된 메모리는 **`Mark`** 합니다. 그 후 **`Mark`** 가 되지 않는 메모리는 사용되고 있지 않는 메모리이기 때문에 **`Sweep`(메모리 해제)** 합니다.

##### Compact 과정
**`Sweep`** 후 분산된 객체들을 힙 영역의 시작 주소로 모아 메모리가 할당된 부분과 할당되지 않은 부분으로 압축하는 과정입니다. 가비지 컬렉터의 종류에 따라 하지 않는 경우도 있습니다.

#### ***Minor GC의 동작방식***
Minor GC는 Young 영역에 대한 가비지 컬렉션입니다. 위에서 Young 영역과 Old 영역의 메모리 구조가 다르다고 했습니다. 아래는 힙 영역의 메모리 구조 입니다.

<p align="center">
  <img width="615" alt="image" src="https://github.com/eello/solve-algorithm/assets/33685064/d62bff4f-6e53-4412-b365-332838a1a54d">

  <br>
  출처: <a href="http://itnovice1.blogspot.com/2019/01/java-garbage-collection.html">IT 내맘대로 끄적끄적</a>
</p>

> Permanent Gerneration은 Java 7까지 있던 영역이며 Java 8부터는 Metaspace로 대체되었습니다. Metaspace는 Heap 메모리 영역이 아니며 OS의 Native 메모리를 사용합니다.

Young 영역은 1개의 Eden 영역과 2개의 Survivor 영역으로 이루어져있습니다.
- Eden : 새로 생성된 객체가 할당(Allocation)되는 영역
- Survivor : 최소 1번의 GC 이상에서 살아남은 객체가 존재하는 영역

Minor GC의 동작방식은 다음과 같습니다.
1. 새로 생성된 객체가 Eden 영역에 할당
2. 객체가 계속 생성되어 Eden 영역이 가득차게 되면 Minor GC 실행
  2-1. Eden 영역에서 사용되지 않는 객체의 메모리가 해제
  2-2. Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동
3. 마찬가지로, 다음 Minor GC 때 Eden 영역에서 사용되지 않는 메모리(참조되지 않는 객체)는 해제되고 사용되는 메모리(참조되는 객체)는 Survivor 영역으로 이동
  - 이때 비어있는 Survivor 영역으로 이동되며 비어있지 않던 나머지 Survivor 영역의 객체도 age가 증가하며 이 영역으로 이동합니다. 비어있던 Survivor 영역은 채워지고 비어있지 않던 Survivor 영역은 비어지게 됩니다.
  - Survivor 영역의 객체들은 서로 다른 age를 갖게됩니다.
4. 1 ~ 3번 과정을 반복하다 객체의 age가 일정 기준을 넘어가게되면 해당 객체는 Old 영역으로 Promote 됩니다.

<p align="center">
  <img width="920" alt="image" src="https://github.com/eello/solve-algorithm/assets/33685064/48eea3ed-e804-49c4-8de2-2cf511cd77ab">

  <br>
  출처: <a href="https://mangkyu.tistory.com/118">망나니개발자</a>
</p>

❗️객체의 age는 객체의 생존 횟수를 의미합니다. 객체의 Header에 기록되며 Minor GC가 끝난 후 일정 age에 도달한 객체는 Old 영역으로 Promote 됩니다.

❗️Survivor 영역은 반드시 한 곳은 비어있는 상태여야 합니다. 두 Survivor 영역에 모두 객체가 존재하거나 모두 비어있다면 현재 시스템이 정상적인 상황이 아님을 파악할 수 있습니다.

HotSpot JVM에서는 Eden 영역에 객체를 빠르게 할당하기 위해 ***`bump the pointer`*** 와 ***`TLABs(Thread-Local Allocation Buffers)`*** 라는 기술을 사용합니다.

> ***HotSpot JVM***
>
> 말 그대로 Hot한 Spot을 찾아 해당 부분에 JIT 컴파일 방식을 사용하는 JVM으로 현재 가장 일반적인 JVM 중 하나입니다.

- ***`bump the pointer`***
  **Eden 영역에 마지막으로 할당된 객체의 주소를 캐싱해두어 새로운 객체를 할당하기 위해 유효한 메모리를 탐색할 필요없이 캐싱된 마지막 주소의 다음 주소를 사용하여 속도를 높이는 기술**입니다. 이를 통해 새로운 객체를 할당할 때 객체의 크기가 Eden 영역에 적합한지만 판별하면 되므로 빠르게 메모리를 할당할 수 있습니다.

- ***`TLABs(Thread-Local Allocation Buffers)`***  
  싱글 쓰레드 환경이라면 문제가 없겠지만 멀티 쓰레드 환경에서는 객체를 Eden 영역에 할당할 때 Lock을 걸어 동기화 해주어야합니다(Eden 영역은 힙 영역에 포함되어있으며 힙 영역은 모든 쓰레드가 공유하는 메모리 공간이므로). 이렇게 Lock을 걸어 동기화하게되면 성능 문제가 발생할 수 있고 이를 해결하기 위해 HotSpot JVM은 TLABs 기술을 도입하였습니다.

  TLABs 기술은 **각각 쓰레드마다 Eden 영역에 객체를 할당하기 위한 주소를 부여함으로써 동기화 작업없이 빠르게 메모리를 할당하도록 하는 기술**입니다. 각각 쓰레드는 자신이 갖는 주소에만 할당함으로써 동기화 없이 bump the pointer를 통해 빠르게 객체를 할당합니다.

#### ***Majr GC 동작방식***
Minor GC를 통해 살아남은 객체들은 age가 올라가고 일정 기준 이상이 되면 Old 영역으로 Promote되게 됩니다. 객체가 계속 Promote되어 ***Old 영역이 가득차게 되면 Old 영역에 대해 Major GC가 실행되며 사용되지 않는 메모리(참조되지 않는 객체)들을 해제하고 공간을 압축합니다.***

Old 영역은 Young 영역보다 크며 Young 영역을 참조할 수 있기 때문에 Minor GC보다 오래걸리게 됩니다. Young 영역과 Old 영역을 동시에 처리하는 GC를 Full GC라고 합니다.

---

참고
- [Oracle - Java Garbage Collection Basics](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
- [망나니개발자](https://mangkyu.tistory.com/118)
- [inpa.tistory.com](https://inpa.tistory.com/entry/JAVA-%E2%98%95-JVM-%EB%82%B4%EB%B6%80-%EA%B5%AC%EC%A1%B0-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%AD-%EC%8B%AC%ED%99%94%ED%8E%B8)
- [코드 연구소](https://code-lab1.tistory.com/92)
- [코딩팩토리](https://coding-factory.tistory.com/829)
- [자바캔](https://javacan.tistory.com/entry/1)
