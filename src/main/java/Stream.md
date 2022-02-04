# java.util.stream.Stream

## 요약
    - 스트림은 순차/병렬 연산/작업을 지원하는 연속된 요소이다.
    - 스트림에는 primitve 타입에 특화된 IntStream, LongStream 등이 있고 객체 참조 수준의 스트림도 있다.
    - 스트림 연산/작업은 스트림 파이프라인으로 구성된다.
    - 스트림은 소스에서 집계한 데이터를 활용해 계산하는데 적합하다.
    - 사용자는 스트림에 대부분의 연산/작업을 선언형(람다/function)으로 제공한다. 그리고 stateless한 함수여야 한다
    - 스트림은 재사용이 불가능하다.

### 영문
```
    A sequence of elements supporting sequential and parallel aggregate operations.
    The following example illustrates an aggregate operation using Stream and IntStream:

    int sum = widgets.stream()
                     .filter(w -> w.getColor() == RED)
                     .mapToInt(w -> w.getWeight())
                     .sum();

    In this example, widgets is a Collection<Widget>.
    We create a stream of Widget objects via Collection.stream(), filter it to produce a stream containing only the red widgets,
    and then transform it into a stream of int values representing the weight of each red widget.
    Then this stream is summed to produce a total weight.

```
### 번역
> 순차, 병렬 집계 연산/작업을 지원하는 요소의 연속이다. <br>
> 아래 예시는 Stream과 IntStream을 사용한 집계 작업이다. <br>

```java
        int sum = widgets.stream()                  // 위젯 컬렉션에 대한 스트림을 생성
                .filter(w -> w.getColor() == RED)   // 위젯의 색이 빨간 색인 요소만 추출
                .mapToInt(w -> w.getWeight())       // 추출된 위젯의 무게를 IntStream으로 변환
                .sum();                             // 스트림 내 요소들의 합을 계산        
```

> 이 예시에서 widgets은 Widget에 대한 컬렉션이다. <br>
> 우리는 Collection.stream을 통해 Widget objects에 대한 스트림을 만들고 빨간색 위젯만 포함하는 스트림을 생성하도록 필터링했다. <br>
> 그리고 각 빨간색 위젯의 무게? 가중치?를 나타내는 IntStream으로 변환했다. 그런 다음 이 스트림의 합을 계산해 전체 무게? 가중치?을 생성했다. <br>
---

### 영문
```
   In addition to Stream, which is a stream of object references,
   there are primitive specializations for IntStream, LongStream, and DoubleStream, all of which are referred to as "streams" and conform to the characteristics and restrictions described here.
```
### 번역
> 객체 참조의 Stream 외에 primitive 타입에 특화된 IntStream, LongStream 및 DoubleStream가 있으며 모두 "Stream"이라고 하며 여기(아래)에 설명되는 특성 및 제한을 따른다.
---

### 영문
```
To perform a computation, stream operations are composed into a stream pipeline.
A stream pipeline consists of a source (which might be an array, a collection, a generator function, an I/O channel, etc),
zero or more intermediate operations (which transform a stream into another stream, such as filter(Predicate)), and a terminal operation (which produces a result or side-effect, such as count() or forEach(Consumer)). Streams are lazy;
computation on the source data is only performed when the terminal operation is initiated, and source elements are consumed only as needed.
```
### 번역
> 계산을 하기 위한 스트림 연산/작업은 스트림 파이프라인으로 구성된다.
> 스트림 파이프 라인은 소스 (소스는 배열, 컬렉션, 생성 함수, I/O 채널, 등이 될 수 있다.)
> 0개 이상의 중간 연산/작업 (필터 (다른 스트림으로 변환), 터미널 연산/작업(count() 또는 foreach))이다.
> 소스 테이터에 대한 계산은 오직 터미널 연산/작업이 시작될 때 수행되고 소스 요소는 필요할 때마다 소비된다.
---

### 영문
```
A stream implementation is permitted significant latitude in optimizing the computation of the result.
For example, a stream implementation is free to elide operations (or entire stages) from a stream pipeline -- and therefore elide invocation of behavioral parameters -- if it can prove that it would not affect the result of the computation.
This means that side-effects of behavioral parameters may not always be executed and should not be relied upon, unless otherwise specified (such as by the terminal operations forEach and forEachOrdered).
(For a specific example of such an optimization, see the API note documented on the count operation. For more detail, see the side-effects section of the stream package documentation.)
```
### 번역
> 스트림 구현체는 결과 계산을 최적화할 때 상당한 자유도가 허용됩니다
> 예를 들어, 스트림 구현체는 스트림 파이프라인에서 계산 결과에 영향을 미치지 않음을 입장할 수 있는 경우 연산/작업(또는 전체 단계)을 생략할 수 있습니다. (행동 매개변수(function)의 호출을 생략.)
> 이것은 행동 매개변수의 사이드 이펙르가 항상 실행되지 않을 수 있다는 것을 의미하며 명시되지 않는 한 실행을 의존해서는 안된다는 것을 의미한다. (예시 : 터미널 작업 (foreach및 forEachOrdered))
> 이러한 최적화의 구체적 예시 : count 연산/작업에 대한 api를 참조해라, Stream documentation의 side-effects를 참조해라.
> https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/package-summary.html
---

### 영문
```
Collections and streams, while bearing some superficial similarities, have different goals.
Collections are primarily concerned with the efficient management of, and access to, their elements.
By contrast, streams do not provide a means to directly access or manipulate their elements, and are instead concerned with declaratively describing their source and the computational operations which will be performed in aggregate on that source.
However, if the provided stream operations do not offer the desired functionality, the iterator() and spliterator() operations can be used to perform a controlled traversal.
```
### 번역
> 컬렉션과 스트림은 표면적으로는 유사하지만 다른 목표를 가지고 있다. 
> 컬렉션은 주로 요소의 효율적인 관리 및 액세스와 관련있다.
> 대조적으로 스트림은 요소에 직접 액세스하거나 조작하는 방법을 제공하지 않는다. 대신 소스에서 집계하여 수행될 계산 작업을 선언적으로 설명하는 데 관련있다.
> 그러나 제공된 스트림 연산/작업이 원하는 기능을 제공하지 않는 경우 iterator, spliterator를 사용하여 제어된 순환을 사용할 수 있다.
---

### 영문
```
A stream pipeline, like the "widgets" example above, can be viewed as a query on the stream source.
Unless the source was explicitly designed for concurrent modification (such as a ConcurrentHashMap), unpredictable or erroneous behavior may result from modifying the stream source while it is being queried.
```
### 번역
> 위의 'widgets'과 같은 스트림 파이프라인은 스트림 소스에 대한 쿼리로 볼 수 있다. 
> 소스가 동시 수정을 위해 설계되지 않는 경우 쿼리 수행 중 스트림 소스를 수정하면 예측할 수 없거나 잘못된 동작이 발생할 수 있다.
---

### 영문
```
Most stream operations accept parameters that describe user-specified behavior, such as the lambda expression w -> w.getWeight() passed to mapToInt in the example above.
To preserve correct behavior, these behavioral parameters:
must be non-interfering (they do not modify the stream source); 
and in most cases must be stateless (their result should not depend on any state that might change during execution of the stream pipeline).
Such parameters are always instances of a functional interface such as Function, and are often lambda expressions or method references. Unless otherwise specified these parameters must be non-null.
```
### 번역
> 대부분의 스트림 연산/작업은 위의 예시에서 mapToInt에 전달된 람다 (w -> w.getWeight())와 같이 사용자 지정 동작을 설명하는 매개변수를 허용한다.
> 올바른 동작을 유지하기 위해 다음 동작 매개변수는 스트림 소스를 수정하지 않아야 합니다.
> 그리고 대부분의 경우 stateless여야 합니다. (결과는 스트림 파이프라인 실행 중 변경될 수 있는 상태에 의존하면 안 됩니다.)
> 이러한 매개변수는 항상 function과 같은 기능 인터페이스의 인스턴스이며 종종 람다 또는 메서드 참조여야 합니다.
> 달리 지정하지 않는한 null이 아니여야 합니다. 
---

### 영문
```
A stream should be operated on (invoking an intermediate or terminal stream operation) only once.
This rules out, for example, "forked" streams, where the same source feeds two or more pipelines, or multiple traversals of the same stream. A stream implementation may throw IllegalStateException if it detects that the stream is being reused. However, since some stream operations may return their receiver rather than a new stream object, it may not be possible to detect reuse in all cases.
Streams have a close() method and implement AutoCloseable. Operating on a stream after it has been closed will throw IllegalStateException. Most stream instances do not actually need to be closed after use, as they are backed by collections, arrays, or generating functions, which require no special resource management. Generally, only streams whose source is an IO channel, such as those returned by Files.lines(Path), will require closing. If a stream does require closing, it must be opened as a resource within a try-with-resources statement or similar control structure to ensure that it is closed promptly after its operations have completed.
Stream pipelines may execute either sequentially or in parallel. This execution mode is a property of the stream.
Streams are created with an initial choice of sequential or parallel execution. (For example, Collection.stream() creates a sequential stream, and Collection.parallelStream() creates a parallel one.) 
This choice of execution mode may be modified by the sequential() or parallel() methods, and may be queried with the isParallel() method.
```
### 번역
> 스트림은 한 번만 동작해야 합니다.
> 예를 들어 동일한 소스가 둘 이상의 파이프 라인에 공급하는 분기된 스트림 or 동일한 스트림의 다중 순회를 제한합니다.
> 스트림이 재사용되고 있음을 감지하면 IllegalStateException이 발생할 수 있다. 그러나 일부 스트림 연산/작업은 새 스트림 구현체가 아닌 receiver를 반환할 수 있어 모든 경우에 재사용을 감지하는 것을 불가능하다.
> 스트림 파이프 라인은 순차적/병렬로 실행할 수 있다. 이 실행 모드는 스트림의 속성(특징)이다.
> 스트림은 순차/병럴 실행의 선택과 함께 생셩됩니다. 예를 들면 Collection.stream()은 순차 스트림을, Collection.parallelStream()은 병렬 스트림을 생성합니다.
> 이러한 실행 모드 선택은  sequential() 또는 parallel()에 의해 수정될 수 있다. 그리고 isParallel()에 의해 (병렬인지 아닌지) 질의될 수 있다. 
---