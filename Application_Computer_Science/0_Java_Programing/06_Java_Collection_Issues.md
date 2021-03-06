# Java Collection Issues
Java에서 Collection 개념은 매우 중요하다.

Collection의 종류는 여러분이 생각한 종류보다 더욱 부지기수하게 존재한다. 

이번 강박사 노트에서는 Collection에 대하여 복습을 할 수 있는 기회를 가져보도록 하겠다.

참고로 여기서 다루는 개념은 기본 개념 보다는 **우리가 여태동안 몰랐던 Collection의 개념**을 주로 다룰 것이다. 기본 개념(예를 들어 메소드 이름과 특징 등)에 대해서는 이미 다 알고 있는 전제 하에 작성하겠다.

## What Is Collection And Map?

![car_parking](/Application_Computer_Science/0_Java_Programing/img/car_parking.jpg)

> Collection 개념은 한 구역에만 차를 주차할 수 있는 엘리베이터 주차장으로 볼 수 있다.

![kinds_trash](/Application_Computer_Science/0_Java_Programing/img/kinds_trash.jpeg)

> Map 개념은 분리수거함과 같은 개념으로 볼 수 있다.

Array의 단점은 고정적인 크기를 고려해야 한다. 예를 들어 아래와 같이 선언했다면 최소 10개의 int 형 변수를 고정적으로만 저장할 수 있다.

```
int[] arr = new int[10];
arr = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
```

하지만 가변적인 데이터의 목록을 어딘가에 모아야 할 필요가 있다. 데이터를 모을 때 순서를 고려해야 하는가, 무작위로 넣어둬도 상관이 없나, 중복된 데이터를 저장할 수 있는가로 나뉘게 된다. 이러한 점을 빌미로 **단일 데이터에 대한 모음**을 구현한 개념을 Collection이라고 한다. 

그렇지만 Collection을 이용할 때 순서를 고려하는 점에서 무조건 자연수(N>0 인 정수, 정수론에서는 0도 가끔 자연수로 취급하는 경우도 있다.)로 취급하는 단점이 있다. 문자열, 객체 등으로 Key 값을 저장할 때 중복된 Value를 저장할 수 있는 개념이 바로 Map이다. 여기서 Collection과 Map의 개념은 별개의 개념으로 생각하고 넘어가야 한다.

## Kinds Of Collection Interface

![Collection_interfaces](/Application_Computer_Science/0_Java_Programing/img/Collection_interfaces.png)

Collection Interface는 3가지로 분류되는데 List, Set, Queue로 나뉘게 된다. 그리고 Map Interface는 별개로 존재한다.

Collection Interface은 데이터의 순서 열람(Iteration)을 반영하기 위해 Iterable Interface로 부터 상속을 받고 이용한다. 반대로 Map Interface에는 순서에 대해 무작위로 저장할 수 있는 Hash Code를 이용해 중복된 Key 값을 배제한다.

각 Interface를 구현한 클래스들 중에서 위의 그림에 있는 Implementation Class들에 대해서만 다루겠다. 이외에도 여러가지가 있는데 이 정도까지만 알아둔다면 큰 약이 된다.

## List
![train_play](/Application_Computer_Science/0_Java_Programing/img/train_play.png)

> List Interface는 데이터의 중복을 떠나서 모든 데이터들이 기차 놀이를 하는 개념으로 볼 수 있다.

List는 데이터를 순서에 맞게 일렬로 구성이 된다. 여기서는 인덱스가 부여되기 때문에 이를 이용한 검색이 가능하면서, 중복을 허용하는 것이 특징이다.

List의 종류는 ArrayList, LinkedList, Vector, Stack 등이 존재한다.

이 사실에 대해서는 자료 구조 시간에 졸지만 않았더라도 이미 다 아는 사실이다. 여기서 더욱 자세하게 어떤 차이점이 있는가를 정리해보도록 하자.

### ArrayList

```
int array_size = 10;
ArrayList<Integer> list1 = new ArrayList<Integer>(array_size);
for(int k=0;k<array_size;k++){
    list1.add(k);
}
list1.add(10); // 여기서는 런타임 오류가 안 나게 된다. 왜 그럴까?

ArrayList<Integer> list2 = new ArrayList<Integer>();
list2.add(10);
list2.add(20); // 일반적으로 우리가 써왔던 ArrayList의 방법은 이렇다.

ArrayList list3 = new ArrayList(); // 기본 사이즈인 10만큼 초기화 된다.
list3.add("가나다라");
list3.add(100);
list3.add(3.14);
list3.add(new Date()); 
// 제네릭을 쓰지 않아도 Object를 넣을 수 있다.
// 그러나 다운 캐스팅에 대한 보장을 할 수가 없다.
```

우리가 ArrayList를 실제로 생성만 해서 계속 추가만 해봤지, 실제로는 데이터의 크기를 고정하고 이용할 수 있는 사실을 잊고 넘어갔었다. 그렇지만 ArrayList에서는 배열의 크기를 초기화한 값만큼 늘려주는 private Method를 함유하고 있다. 이 Method를 `grow()`라고 한다. 

예를 들어 ArrayList의 크기를 10으로 초기화하고 10개의 데이터를 추가하고 난 다음에 새로운 데이터를 또 추가한다면, 크기의 한계를 벗어나기 위해 11로 알아서 1씩 키운다. 

ArrayList의 크기가 동적으로 추가되거나 삭제할 때 그 인덱스 다음에 있는 데이터를 밀거나 당기는 경우의 성능 저하의 발생 여부로 LinkedList보다 느릴 수도 있다는 사실을 생각해봐야 한다. 이 때 내부에서 이용하는 메소드가 바로 `System.arraycopy()`이다. 인스턴스를 생성할 필요가 없는 장점이 있어 메모리 자원 낭비는 막아주지만, 속도에 대해서는 보장을 못 하기 때문에 결국에는 LinkedList보다 느릴 수 밖에 없다.

또한 ArrayList 내부에 배열이란 자체가 존재해서 인덱스를 이용할 때 더욱 효과적인 List로도 뽑힌다.

### LinkedList

```
LinkedList<String> list1 = new LinkedList<String>();
list1.add("10"); // Node를 추가하고
list1.add("30"); // Node를 추가하고
list1.add("50"); // Node를 또 추가한다.
list1.add("70"); // Node를 계속 추가한다.
 
list1.contains("70"); // 이 때 Node를 얼마나 더 순회해야 할까?
list1.get(3); // 이 때 Node를 얼마나 더 순회해야 할까?
```

LinkedList는 ArrayList와 달리 배열이라는 자체가 존재하지 않아서 데이터 탐색에서 모든 데이터를 순회하고 봐야 하기 때문에 시간 복잡도는 O(n)인 점과 비교한다면 ArrayList(배열의 인덱스를 따오면 되니까 O(1))보다 비효율적이다.

그렇지만 LinkedList 만의 매력이 하나 있다면 데이터의 Index에 상관 없이 그 자리 이전 Node와 그 자리 이후 Node를 제대로 정리하면 되는 점에서 시간 복잡도가 O(1)로 나오게끔 구현이 되어 있어서 데이터 삽입, 삭제에서는 ArrayList보다 우월한 편이다. 

우리가 자료구조 시간에 LinkedList를 구현할 때 Index 별로 순회를 하고 추가, 삭제하는 것과는 달리 `Node Vertex`가 존재하여 이에 따른 작업을 진행하기 때문에 이전 Node와 이후 Node를 따지는 것만 기억해두면 된다.

또한 LinkedList는 Stack, Queue, Deque에서 데이터 삽입, 삭제 시에 Head와 Tail쪽에서 이뤄질 때 가장 적합한 자료구조가 바로 LinkedList이다. ArrayList와 비교한다면 다른 자료구조와의 확장성이 뛰어나다. 자세한 내용은 후술하겠다.

> **ArrayList VS LinkedList**
> 
> ArrayList와 LinkedList 둘 다 Serializable, Cloneable, Iterator Interface를 따와서 구현이 되어 있는 공통점이 있어서 이에 대해서 신경쓰진 않아도 된다.
> 
> 하지만 부지기수한 데이터에 따른다면 ArrayList와 LinkedList 중에서 어느 것을 써야 더욱 효과적일까? 
> 
> 아직까지 짜장면과 짬뽕 중 무엇을 먹을 것인가의 Issue와 같을 수도 있는데 데이터 변동 사항이 적은 경우에 탐색을 보장하는 측에서는 ArrayList를 사용하는 것이 효과적이고, 데이터 변동 사항이 비록 많지만 탐색을 하는 빈도가 적은 경우에 LinkedList를 쓰는 것이 좋은 방법이다.
> 
> 평소에는 ArrayList를 더욱 많이 쓰는 편이지만, 데이터가 많아지면 일일히 배열에 저장하기 때문에 공간 복잡도가 O(N)으로 커지는 단점이 있다. 즉 데이터 양이 적으면 ArrayList를 쓰는 것이 효율적이다.
> 
> 그렇지만 데이터 변동을 처음부터 끝까지 데이터를 탐색하면서 함께 작업하는 경우에는 LinkedList를 사용하는 것이 좋다.
> 
> 더욱 자세한 스펙은 아래 링크를 참고하면 좋겠다. 
> 
> http://javaconceptoftheday.com/arraylist-vs-linkedlist-java/

### Vector

```
Vector<Integer> vector = new Vector<Integer>(5);
vector.add(10);
vector.add(20);
...
// 50까지 추가했다면 ArrayList처럼 배열 크기를 알아서 늘리겠지...
vector.add(60);
vector.add(70);
...
// 여기까지만 두면 ArrayList와 별반 차이가 없는데??
```
> Java 1 버전부터 List 개념 조차 없었을 때 태어난 조상 격 개념.

Vector도 ArrayList와 마찬가지로 가변 길이 배열에 대응하기 위한 개념으로 볼 수 있다. ArrayList처럼 초기화했던 배열의 길이보다 커진다면 알아서 길이를 늘리는 개념과 다를 바가 전혀 없다. 도대체 ArrayList와는 무슨 차이가 존재하는 것일까?

Vector는 ArrayList와 달리 동기화를 보장하는 면에서 차이점을 볼 수 있다. ArrayList는 Singled-Thread에서 효율적으로 작동할 수 있게 구성한 자료구조이지만, Vector는 Multi-Thread에서 여러 데이터들이 각 다른 Thread 별로 요청이 들어온다면 그 순서에 맞춰 처리하는 특성을 가지고 있다.

이러한 이유는 ArrayList의 크기를 이미 범람하고 난 후에 데이터를 추가할 때 1씩 증가할 때 계속 들어오는 데이터에 대해 `grow()` 작업을 하는 면에서 비효율적이지만, Vector는 반대로 처음에 초기화한 배열의 크기 만큼 Capacity(용량)를 더욱 크게 잡는 면에서 동기화를 그나마 보장할 수 있기 때문이다.

이외의 차이점은 크게 없지만, 동기화를 보장하는 작업과 함께 이뤄지는 면에서 ArrayList보다는 Vector를 사용하는 것이 더욱 좋다.

### Stack

```
// Case 1번과 2번 동일한 방법이다.
// Case 1. Stack 클래스 이용
Stack<Integer> stack = new Stack<Integer>();
stack.push(10);
stack.push(20);
stack.push(30);

stack.peek(); // 30 반환

stack.pop(); // 30 반환 및 삭제
stack.pop(); // 20 반환 및 삭제

// Case 2. LinkedList 클래스 이용
LinkedList<Integer> stackList = new LinkedList<Integer>();
stackList.addLast(10);
stackList.addLast(20);
stackList.addLast(30);

stackList.peekLast();

stackList.removeLast();
stackList.removeLast();
```

Stack은 자료구조 시간에 `LIFO(Last In First Out)` 개념을 까먹지 않았다면 이미 다 아는 사실이다. Stack 개념은 백트래킹, DFS, 재귀함수 등에서 많이 쓰이는 개념이지만, Stack의 길이는 확실히 정해지지 않았다는 것을 알 수 있다. 그래서 Java Collection에서 Stack은 LinkedList를 기반으로 구현했다는 사실을 알 수 있다.

추가로 peek() 함수는 Stack 내부에 현재 맨 마지막에 있는 데이터를 읽어 들이기만 한다. 이외의 push(), pop() 함수는 똑같다.

> 참고로 1번 방법과 2번 방법은 유사하지만, 성능 면에서는 어떤 차이가 있는지 아시는 분은 Issues에 제보 해주시면 감사하겠습니다. 적극 반영 하겠습니다. 😊

## Set

![diagram](/Application_Computer_Science/0_Java_Programing/img/diagram.jpg)

> 수학의 집합을 데이터로 표현한 것이 바로 Set의 개념이다.

Set는 List와 달리 순서를 정하면서 데이터를 추가하는 것은 둘 째로 치고, **데이터의 중복을 방지**하기 위한 자료 구조로 볼 수 있다.

Set의 종류로는 HashSet, TreeSet, LinkedHashSet 등이 있다.

Set의 이용 사례로 그래프 정점 별로 방문 처리 체크를 할 때 이용될 수 있다.

참고로 Set에 대해서는 동기화를 제공하지 않는 단점이 있기 때문에 동기화를 적용하기 위해서는 `Collections.synchronizedSet([SET 객체 인스턴스])`를 이용해야 한다.

### HashSet

```
HashSet<String> persons = new HashSet<String>(); // *
persons.add("그사람");
persons.add("저사람");
persons.add("이사람");

Iterator<String> iterator = persons.iterator();
while(iterator.hasNext()) {
    String temp = iterator.next();
    System.out.println(temp); 
    // 어떤 것들이 먼저 나올지 예측할 수 없다.
}
```

HashSet는 HashCode를 기반으로 중복되지 않은 데이터를 저장할 때 사용한다. HashSet에 데이터 중복 확인을 위해서 객체 내부에 hashCode(), equals() 메소드가 포함되어 있어야 한다. 또한 null 값도 추가할 수 있다. 또한 Iterator를 이용해서 그 안에 있는 값들로 모든 데이터를 순회할 수 있지만, 순서에 대해서는 HashCode를 알지라도 예측이 불가능하다.

HashSet의 데이터 삽입, 삭제 시간 복잡도는 HashCode를 기반으로 삽입, 삭제가 진행되기 때문에 O(1)이 나오게 된다. 또한 이 내부에 있는 데이터 중의 일부를 수정할 수 없다.

그리고 *처럼 체크 된 부분에서 HashSet의 용량은 16, 적중률이 75%로 초기화가 되는데 적중률은 HashSet의 용량의 3/4 만큼 데이터가 저장 된다면 HashCode의 계산이 다시 진행 된다는 뜻이다.

### TreeSet

![RBTree](/Application_Computer_Science/0_Java_Programing/img/RBTree.png)

```
TreeSet<String> persons = new TreeSet<String>();
persons.add("이사람");
persons.add("저사람");
persons.add("그사람");
persons.add("거사람");
System.out.println(persons); // [거사람, 그사람, 이사람, 저사람]
```

TreeSet는 Red-Black Tree을 이용해 데이터의 중복을 방지하는 것과 동시에 HashCode 오름차순으로 추출할 수 있는 Sorted Sort의 일부이다. Red-Black Tree에서 데이터 삽입, 삭제의 시간 복잡도는 Red-Black Tree 법칙에 따른 순서 조정으로 인하여 Node 수가 적더라도 O(log n)이 나온다. 그렇지만 HashCode에 따라서 데이터를 정렬하기 때문에 오름차순으로 정리 된 데이터로 집합을 구성할 필요가 있다면 TreeSet가 효율적이다.

또한 TreeSet도 마찬가지로 Iterator를 가져올 수 있는데 HashCode를 기준으로 오름차순으로 정렬되어 반환한다.

참고로 TreeSet의 저장 순서를 내림차순으로도 바꿀 수 있는데 이는 Comparable<T> Interface를 이용해서 구현해야 가능한 일이다. 이는 Lambda 식으로도 가능하다.

### LinkedHashSet

```
LinkedHashSet<Integer> list01 = new LinkedHashSet<Integer>();
list01.add(4);
list01.add(2);
list01.add(6);

// list01 : [4, 2, 6]

List<Integer> sorted = new ArrayList<Integer>(list01);
Collections.sort(sorted);
```
> HashSet와 LinkedList의 일부 개념을 서로 합친 자료구조

HashSet에서는 순서와 상관 없이 데이터가 저장되고, TreeSet는 데이터를 넣으면 내부의 Tree를 이용해서 정리를 하면서 오름차순으로 저장이 된다. 

그렇지만 Set도 넣은 데이터 순서를 유지할 때 성능은 보장 못 하더라도 쓸 수 있는 자료구조가 있는데 바로 LinkedHashSet이다. LinkedList의 시간 복잡도는 O(1)로 TreeSet보다 빠른 편이다. 

LinkedList에서는 데이터의 중복을 허가하지만, LinkedHashSet는 Set인 만큼 중복은 씨도 안 먹힌다. LinkedList에서는 데이터를 탐색할 때 시간 복잡도가 O(n)으로 나오는 단점이 있지만, LinkedHashSet는 데이터 포함 여부만 HashCode를 이용해서 계산하면 되니 어떻게 보면 LinkedList보다 효율적인 면을 제공한다.

LinkedHashSet는 다만 **데이터를 넣은 순서**대로 반영이 되기 때문에 정렬 기능을 제공하지 않는다. 정렬을 제공하는 자료구조는 List 밖에 없기 때문에 List를 이용한 초기화 작업을 거치고 난 후에 정렬해야 한다. 이 뿐만 아니라 HashSet, TreeSet(오름 차순 이외에 내림 차순을 하는 경우)도 마찬가지이다.

## Queue

![queue](/Application_Computer_Science/0_Java_Programing/img/queue.png)

Queue의 개념은 자료 구조 시간에 이미 다 숙달했을 것이다. 은행이나 영화관, 유명 식당 등에서 번호표를 뽑아서 순서대로 입장하는 방식이 Queue의 FIFO 원리를 실생활에서 볼 수 있는 간단한 사례로 볼 수 있다.

Queue를 구현할 수 있는 자료구조로는 LinkedList가 제격이다. Array로 Queue를 구현하는 것은 Circular Array Queue를 이용하면 되지만 처리하는 시간 복잡도만 늘지 별로 얻어지는 이득은 없다.

LinkedList에서는 Doubly LinkedList(이중 연결 리스트)로 구현이 되어 있어서 Deque(덱, 양방향 Queue)도 문제 없이 작동된다.

### LinkedList With Queue

```
Queue<Integer> bfs = new LinkedList<Integer>();
bfs.add(10);
while(!bfs.isEmpty()){
    int tmp = bfs.poll();
    // bfs 작업 구현
}
```

Queue 자체를 이용하기 위해서는 LinkedList를 말미암아 사용해야 한다. 이 사실에 대해서는 위에서도 언급했기 때문에 크게 설명하지 않고 넘어가겠다.

그렇지만 Queue의 연산자는 enQueue(T), deQueue()이지만 여기서는 offer(T)와 poll() 함수를 쓴다. 또 한 편으로는 add(T)와 remove() 함수를 이용하는 경우가 있는데 이 둘의 차이는 `NoSuchElementException` 처리 여부로 볼 수 있다. add(T), remove() 함수에서는 Queue 내부에 데이터가 없다면 `NoSuchElementException`을 반환하게 되지만, offer(T)와 poll() 함수는 null을 반환한다. 이 때 받아온 데이터를 참조할 때 `NullPointerException` 판단 여부를 고려할 필요가 있다.

### ArrayDeque

```
ArrayDeque<Integer> deque = new ArrayDeque<Integer>();
deque.offerFirst(10);
deque.offerFirst(20);
// deque = [20, 10]
deque.offerLast(30);
deque.offerLast(40);
// deque = [20, 10, 30, 40]
```

양방향 Queue를 구현한 개념을 Deque(덱)이라고 한다. 이 내부에는 이미 짐작을 했겠지만 자료구조 내부에서 알아서 크기를 조정하는 배열이 들어 있어서 Index를 이용한 조회에서는 빠르게 느낄 수도 있지만, Deque의 연산 그대로 Head에서 enQueue(offerFirst), deQueue(pollFirst), Tail에서 enQueue(offerLast), deQueue(pollLast) 하는 것 이외에는 없다.

ArrayDeque를 한다면 LinkedList처럼 Head와 Tail에서 삽입, 삭제가 일어나기 때문에 데이터 변동에 대한 걱정은 딱히 없다. 다만 null 값을 추가할 수 없다. 짐작할 수 있는 사실은 List와 Set에서 null 값을 넣는 건 딱히 상관 없는 일이지만, 여기서는 Deque가 비어있을 때 null 값을 반환하는 것으로 약속이 되어 있기 때문에 애초에 ArrayDeque를 만들 때 작정한 걸로 짐작이 된다.

Deque Interface가 모두 null 값을 못 넣게 막은 것은 아니다. 여기서만 특별히 null 값을 못 넣게 막은 것이므로 이 정도 Issue에 대해서는 잠깐 읽어볼 필요도 어느 정도 있다.

### PriorityQueue

![heapOrder](/Application_Computer_Science/0_Java_Programing/img/heapOrder.png)

```
PriorityQueue<Integer> queue = new PriorityQueue<Integer>();
queue.offer(30);
queue.offer(3);
queue.offer(3000);
queue.offer(300);

while(!queue.isEmpty()){
    System.out.println(queue.poll());
}
// 결과는 3, 30, 300, 3000으로 오름차순으로 출력된다.
```

> - PriorityQueue == Heap ? 절대로 그렇지 않다.<br/>
> - PriorityQueue != Heap ! 그렇다.<br/>
> - Heap은 자료구조의 개념이지만, Priority Queue는 ADT(Abstract Data Type, 추상적 데이터 타입)이다.

Priority Queue에서 짚고 넘어가야 하는 개념이 바로 ADT 개념이다. ADT의 주요 목적은 **어떤 일을 할 수 있다** 의 초점을 두고 무엇이 어떻게 들어가는지 관심 조차 없음과 얼마의 시간이 걸리는지에 대해서도 관심이 없다.

이처럼 Priority Queue에서는 임의로 데이터를 꺼내는 것이 아닌 가장 높은 우선순위의 것을 뽑는 점을 빌미로 **시간이나 공간 제약을 두지 않고** 한정된 수의 요구사항을 명확히 잡고 어떤 방법으로 구현하는 것이 효율적인가를 생각하는 점을 고려하고 있다.

Priority Queue에는 가중치의 최댓값 혹은 최솟값을 가져올 때 필요한 Heap 개념이 숨어 있다. Heap 자료구조를 처음에 설정할 때는 최대 힙, 최소 힙에 따른 정책이 이미 정해져서 내부를 설정해야 된다. 그렇지만 Priority Queue에서는 객체의 Comparable<T> 관련 메소드인 `compareTo(T)`를 따로 구현하여 설정하면 된다.

예를 들어 간략하게 도시 인구에 따른 내림차순을 반환하는 사례로 소스 코드를 작성해보도록 하겠다.

```
class City implements Comparable<City>{
    String name;
    int popular;

    public City(String name, int popular){
        this.name=name;
        this.popular=popular;
    }

    @Override
    public int compareTo(City city){
        if(this.popular > city.popular) return -1;
        else if(this.popular < city.popular) return 1;
        else return 0;
    }
}
public class Main{
    public static void main(String[] args){
        PriorityQueue<City> cities = new PriorityQueue<>();
        cities.offer(new City("Seoul", 10000000));
        cities.offer(new City("Daejeon", 2500000));
        cities.offer(new City("Busan", 4500000));
        cities.offer(new City("Incheon", 3000000));
        cities.offer(new City("Daegu", 2200000));

        while(!cities.isEmpty()){
            City city = cities.poll();
            System.out.println(String.format("{name : %s - popular : %d}", city.name, city.popular));
        }
    }
}
```

```
{name : Seoul - popular : 10000000}
{name : Busan - popular : 4500000}
{name : Incheon - popular : 3000000}
{name : Daejeon - popular : 2500000}
{name : Daegu - popular : 2200000}
```

어떻게 보면 Queue 처럼 쓰는 방법이랑 똑같지만, 데이터를 삽입(offer), 삭제(poll)하는 경우에는 내부에 있는 Heap의 조정이 필요해 Heapify가 이뤄지게 되는데 이 시간 복잡도가 O(log n)이 걸리게 된다. 그렇지만 값(Object)을 이용한 삭제나 포함 여부(contains) 함수를 돌리는 시간 복잡도는 O(n)으로 선형 시간이 걸리게 된다.

이 자료구조는 실제 알고리즘 코딩 테스트에서도 잘만 활용한다면 약이 될 수 있는 자료구조이기 때문에 알아두고 넘어가자.

## Map

Map Interface는 Collection Interface와 **별개의 개념이다.** Map에서는 List 처럼 데이터의 순서가 정해진 것도 아니고, 오로지 Key와 Value를 이용해서 얻을 뿐이다. 어떻게 보면 경계 근무에서 쓰이는 암구호와 같은 원리로 볼 수 있다. 문어가 Key라면, 답어가 Value이다.

여기서도 마찬가지로 사용 방법보다는 성능에 대한 Issue를 더 두고 작성하겠다. 

### HashMap

```
HashMap<String, String> map = new HashMap<String, String>(); // 초기화
map.put("A", "alpha");
map.put("B", "beta");
map.put("O", "omega");
map.put("D", "delta");

// 저장 순서는 추측할 수 없다.
```

HashMap은 자체 내부에 Hash Table(해시 테이블) 원리를 접목시켰기 때문에 데이터의 저장 순서에 대해 크게 신경을 쓰지 않고, 오로지 유일한 Key 값을 이용해서 Value를 저장하는 방법이다. 물론 Key 값을 null로 지정해도 문제는 없다.

HashMap에 저장된 Key 값들과 Value 값들의 목록을 Set로 정리 가능하다. 그리고 Key, Value의 짬뽕을 Entry로 묶어서 가져오는 방법 또한 가능하다.

HashMap의 시간 복잡도는 데이터 삽입, 삭제, 검색 모든 시간 복잡도는 O(1). 상수 시간 복잡도로 걸린다.

HashSet와 마찬가지로 주석에 초기화로 되어 있는 것처럼 초기화 한다면 용량은 16, 적중률을 75%로 체크하여 HashMap의 사이즈가 커지면서 적중률을 넘는 경우에 HashCode를 다시 계산한다.

### LinkedHashMap

```
LinkedHashMap<String, String> map = new LinkedHashMap<String, String>(); // 초기화
map.put("A", "alpha");
map.put("B", "beta");
map.put("O", "omega");
map.put("D", "delta");
// 실행 결과는 {A=alpha, B=beta, O=omega, D=delta}.
```

> LinkedHashSet 와 도찐개찐.<br/> 
> HashMap에 Linked 개념까지 포함시켜 HashMap에서 데이터를 넣은 순서를 반영한다.

사진에는 HashLinkedMap으로 작성되어 있는데 **LinkedHashMap** 이 맞다.

LinkedHashMap은 HashMap에 넣은 순서를 더욱 보장하기 위해 쓰는 자료구조로 볼 수 있다. LinkedHashMap은 HashMap과의 시간 복잡도를 비교한다면 데이터 삽입, 삭제에서는 큰 문제가 없지만, 데이터 탐색에서는 O(n)를 가진다.

또한 LinkedHashMap에서 제공하는 또 하나의 메소드로 removeEldestEntry(Entry<K,V>)가 있는데 이는 들어온 순서를 기억하고 제한 크기에 맞춰서 자를 때 사용하는 메소드이다. 예를 들어 Map의 크기를 5개로 자른다면, Key 값이 A, B, C, D, E, F 6개로 들어올 때 결국 A가 없어지고 B부터 F까지 남는다. 이러한 방안은 Cache 교체 전략 알고리즘(LRU, LFU, FIFO)을 구현할 때 참고하면 좋다.

### TreeMap

```
TreeMap<String, String> map = new TreeMap<String, String>(); // 초기화
map.put("A", "alpha");
map.put("O", "omega");
map.put("D", "delta");
map.put("B", "beta");
// 실행 결과는 {A=alpha, B=beta, O=omega, D=delta}.
```

> TreeSet 와 도찐개찐.<br/> 
> Map에 Red-Black Tree를 박아 둔 개념.

TreeMap도 솔직히 이야기한다면 Red-Black Tree를 박아둔 거와 똑같다. 그래서 데이터를 추가, 삭제할 때 걸리는 시간 복잡도는 O(log n)으로 웬만한 Map들 보다 성능이 형편 없다. 그렇지만 Key 값 별로 오름 / 내림 차순으로 정리하기 때문에 Key 값에 대해 Index로 정렬할 필요가 있다면 이 자료구조를 적용하는 것이 좋다.

### Hashtable

```
Hashtable<String, String> map = new Hashtable<String, String>(); // 초기화
map.put("A", "alpha");
map.put("B", "beta");
map.put("O", "omega");
map.put("D", "delta");

// 저장 순서는 Multi-Thread 별로 달라지기 때문에 추측이 불가능하다.
```

Hashtable은 어떻게 보면 HashMap과 별 반 다를 게 없어 보인다. 그렇지만 HashMap은 애초에 동기화를 보장하지 않은 환경에서 써야 한다. 반대로 Hashtable은 동기화를 보장할 수 있게 도와준다.

HashMap은 동시에 접근하는 환경에서 적합하지 않고 차라리 ConcurrentHashMap를 이용하는 것을 추천하고 있다. 물론 Map Interface에도 동기화를 보장하는 함수가 존재한다.

ConcurrentHashMap과 Hashtable의 공통점은 Key 값에 null 값을 절대로 못 넣는다. HashMap에는 null 값을 넣어 매꾸는 일이 가능하지만, 동기화를 보장하는 과정에서 null 값을 넣는다면 Multi Thread에서 공통적으로 들어오는 null 값에 대해 겹칠 수 있기 때문이다.

Hashtable의 시간 복잡도는 HashMap과 같지만, 각 메소드에 synchronized 키워드가 포함된 점에서 단일 Thread의 작업에선 비효율적이다.

## Iterator


## References
- http://egloos.zum.com/dojeun/v/317868 - Collection 개념에 대해 간략하게 정리된 사이트
- https://blog.naver.com/windziel/60048694876 - Collection 개념에 대하여 상세하게 잘 정리된 사이트
- https://brunch.co.kr/@springboot/57 - Collection 개념에 대하여 미처 알지 못한 개념들을 다룰 때 좋은 사이트