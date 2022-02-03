# java.util.stream.Stream


```
    A sequence of elements supporting sequential and parallel aggregate operations.
    The following example illustrates an aggregate operation using Stream and IntStream:

    int sum = widgets.stream()
                     .filter(w -> w.getColor() == RED)
                     .mapToInt(w -> w.getWeight())
                     .sum();

    In this example, widgets is a Collection<Widget>.
    We create a stream of Widget objects via Collection.stream(), filter it to produce a stream containing only the red widgets, and then transform it into a stream of int values representing the weight of each red widget.
    Then this stream is summed to produce a total weight.


    
    순차, 병렬 집계 연산/작업을 지원하는 요소의 연속이다.
    아래 예시는 Stream과 IntStream을 사용한 집계 작업이다.

    int sum = widgets.stream()                          // 위젯 컬렉션에 대한 스트림을 생성
                     .filter(w -> w.getColor() == RED)  // 위젯의 색이 빨간 색인 요소만 추출
                     .mapToInt(w -> w.getWeight())      // 추출된 위젯의 무게를 IntStream으로 변환
                     .sum();                            // 스트림 내 요소들의 합을 계산

    이 예시에서 widgets은 Widget에 대한 컬렉션이다.
    우리는 Collection.stream을 통해 Widget objects에 대한 스트림을 만들고 빨간색 위젯만 포함하는 스트림을 생성하도록 필터링했다.
    그리고 각 빨간색 위젯의 무게? 가중치?를 나타내는 IntStream으로 변환했다. 그런 다음 이 스트림의 합을 계산해 전체 무게? 가중치?을 생성했다.
```

```
   In addition to Stream, which is a stream of object references,
   there are primitive specializations for IntStream, LongStream, and DoubleStream, all of which are referred to as "streams" and conform to the characteristics and restrictions described here.



   객체 참조의 Stream 외에 primitive 타입에 특화된 IntStream, LongStream 및 DoubleStream가 있으며 모두 "Stream"이라고 하며 여기(아래)에 설명되는 특성 및 제한을 따른다.
```


```
To perform a computation, stream operations are composed into a stream pipeline. A stream pipeline consists of a source (which might be an array, a collection, a generator function, an I/O channel, etc), zero or more intermediate operations (which transform a stream into another stream, such as filter(Predicate)), and a terminal operation (which produces a result or side-effect, such as count() or forEach(Consumer)). Streams are lazy; computation on the source data is only performed when the terminal operation is initiated, and source elements are consumed only as needed.
A stream implementation is permitted significant latitude in optimizing the computation of the result. For example, a stream implementation is free to elide operations (or entire stages) from a stream pipeline -- and therefore elide invocation of behavioral parameters -- if it can prove that it would not affect the result of the computation. This means that side-effects of behavioral parameters may not always be executed and should not be relied upon, unless otherwise specified (such as by the terminal operations forEach and forEachOrdered). (For a specific example of such an optimization, see the API note documented on the count operation. For more detail, see the side-effects section of the stream package documentation.)
Collections and streams, while bearing some superficial similarities, have different goals. Collections are primarily concerned with the efficient management of, and access to, their elements. By contrast, streams do not provide a means to directly access or manipulate their elements, and are instead concerned with declaratively describing their source and the computational operations which will be performed in aggregate on that source. However, if the provided stream operations do not offer the desired functionality, the iterator() and spliterator() operations can be used to perform a controlled traversal.
A stream pipeline, like the "widgets" example above, can be viewed as a query on the stream source. Unless the source was explicitly designed for concurrent modification (such as a ConcurrentHashMap), unpredictable or erroneous behavior may result from modifying the stream source while it is being queried.
Most stream operations accept parameters that describe user-specified behavior, such as the lambda expression w -> w.getWeight() passed to mapToInt in the example above. To preserve correct behavior, these behavioral parameters:
must be non-interfering (they do not modify the stream source); and
in most cases must be stateless (their result should not depend on any state that might change during execution of the stream pipeline).
Such parameters are always instances of a functional interface such as Function, and are often lambda expressions or method references. Unless otherwise specified these parameters must be non-null.
A stream should be operated on (invoking an intermediate or terminal stream operation) only once. This rules out, for example, "forked" streams, where the same source feeds two or more pipelines, or multiple traversals of the same stream. A stream implementation may throw IllegalStateException if it detects that the stream is being reused. However, since some stream operations may return their receiver rather than a new stream object, it may not be possible to detect reuse in all cases.
Streams have a close() method and implement AutoCloseable. Operating on a stream after it has been closed will throw IllegalStateException. Most stream instances do not actually need to be closed after use, as they are backed by collections, arrays, or generating functions, which require no special resource management. Generally, only streams whose source is an IO channel, such as those returned by Files.lines(Path), will require closing. If a stream does require closing, it must be opened as a resource within a try-with-resources statement or similar control structure to ensure that it is closed promptly after its operations have completed.
Stream pipelines may execute either sequentially or in parallel. This execution mode is a property of the stream. Streams are created with an initial choice of sequential or parallel execution. (For example, Collection.stream() creates a sequential stream, and Collection.parallelStream() creates a parallel one.) This choice of execution mode may be modified by the sequential() or parallel() methods, and may be queried with the isParallel() method.
```