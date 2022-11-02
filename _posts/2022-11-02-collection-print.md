---
layout: post
categories: "Java"
title: "Java Collection Print"
author: "TY_K"
---

# 자주 쓰는 Java collections의 출력 방법

## Array

```java
int[] arr = { 1, 2, 3, 4, 5 };
 
System.out.println(arr);
// [I@762efe5d
```

array의 경우 그냥 출력 하면 원하는 값이 아닌, 메모리 주소가 출력된다.

### 반복문 이용하기

```java
int[] arr = { 1, 2, 3, 4, 5 };
 
for (int i = 0; i < arr.length; i++) {
  System.out.print(arr[i] + ' ');
}
// 1 2 3 4 5 
```

### java.util.Arrays의 toString() 이용하기

```java
int[] arr = { 1, 2, 3, 4, 5 };
 
System.out.println(Arrays.toString(arr));
// [1,2,3,4,5]
```

## Map

### map.entrySet()

```java
// HashMap 준비
Map<Integer, String> map = new HashMap<Integer, String>();
map.put(1, "Apple");
map.put(2, "Banana");
map.put(3, "Orange");

// for loop (entrySet())
for (Entry<Integer, String> entrySet : map.entrySet()) {
  System.out.println(entrySet.getKey() + " : " + entrySet.getValue());
}
// 1 : Apple
// 2 : Banana
// 3 : Orange
```

### map.keySet(), mep.get()

```java
// HashMap 준비
Map<Integer, String> map = new HashMap<Integer, String>();
map.put(1, "Apple");
map.put(2, "Banana");
map.put(3, "Orange");

// for loop (keySet())
Set<Integer> keySet = map.keySet();
for (Integer key : keySet) {
  System.out.println(key + " : " + map.get(key));
}
// 1 : Apple
// 2 : Banana
// 3 : Orange
```

### map.values()

```java
// HashMap 준비
Map<Integer, String> map = new HashMap<Integer, String>();
map.put(1, "Apple");
map.put(2, "Banana");
map.put(3, "Orange");

// map.values()
Collection<String> values = map.values();
System.out.println(values);
// [Apple, Banana, Orange]
```

### forEach (Java 8 이후)

```java
// HashMap 준비
Map<Integer, String> map = new HashMap<Integer, String>();
map.put(1, "Apple");
map.put(2, "Banana");
map.put(3, "Orange");

// forEach
map.forEach((key, value) -> {
  System.out.println(key + " : " + value);
});
// 1 : Apple
// 2 : Banana
// 3 : Orange
```

## Iterator

Iterator은 3가지 메서드를 제공한다.
- hasNext()
- next()
- remove()

```java
ArrayList<String> list = new ArrayList<>();

list.add("Apple");
list.add("Banana");
list.add("Orange");

Iterator<String> itr = list.iterator();

while(itr.hasNext()){
  System.out.println(itr.next());
}
// Apple
// Banana
// 가을
```

```java
// HashMap 준비
Map<Integer, String> map = new HashMap<Integer, String>();
map.put(1, "Apple");
map.put(2, "Banana");
map.put(3, "Orange");

// Iterator
Iterator<Entry<Integer,String>> it = map.entrySet().iterator();

while(it.hasNext()) {
  Entry<Integer, String> entrySet = (Entry<Integer, String>) it.next();
  // key, value 출력
  System.out.println(entrySet.getKey() + " : " + entrySet.getValue());
}
// 1 : Apple
// 2 : Banana
// 3 : Orange
```

#### 원문링크

[[Java] 배열 값 출력하는 2가지 방법 (반복문, Arrays.toString())][link1]

[link1]: https://hianna.tistory.com/510 "link1"

[[Java] HashMap key, value 전체 출력하기][link2]

[link2]: https://hianna.tistory.com/573 "link2"

[[Java] Iterator(반복자)][link3]

[link3]: https://tragramming.tistory.com/100 "link3"



