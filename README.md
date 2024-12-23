# Java Refresher


## Empty array

`int[] emptyArray = new int[0];` , but this won't even compile `int[] compileError = new int[];`

## Instant
```java
import java.time.Instant;
public class Test {
	public static void main(String[] args) {
		Instant now = Instant.now();
		System.out.println("Now : "+now); // Now : 2024-11-08T16:10:02.367923Z
		long millis = now.toEpochMilli();
		System.out.println("Milis from Epoch :"+millis);
		// Milis from Epoch :1731082202367
	}
}
```
## ArrayDeque Vs. LinkedList

`java.util.ArrayDeque` doesn't allow null while `java.util.LinkedList` does allow adding null.

## Cloning a 1D/2D array of primitives with .clone()
```java
    int[] arr1 = {5, 6, 3, 2, 10};
    int[] arr2 =  arr1.clone();
    System.out.println("Arr 2 : " + Arrays.toString(arr2)); // {5, 6, 3, 2, 10}
    int[][] arr3 = {{1, 2, 3}, {4, 5, 6}};
    int[][] arr4 = arr3.clone();
    for(int[] nums : arr4)
    System.out.println("Arr 4 : " + Arrays.toString(nums));  // {{1, 2, 3}, {4, 5, 6}}
```
## HashMap does not guarantee any particular order of keys
```java
import java.util.*;

public class Test {
    public static void main(String[] args) {
       Map<Integer,String> map = new HashMap<>();
       map.put(1, "one"); map.put(3, "three");
       map.put(2, "two"); map.put(5, "five");
       map.put(6, "six"); map.put(4,  "four");
       for(int key : map.keySet())
    	   System.out.print(key+" , "); // the output order can't be guaranteed
       // 1 , 2 , 3 , 4 , 5 , 6 , 
    }
}
```

## Use LinkedHashMap if you want to maintain insertion order
```java
import java.util.*;

public class Test {
    public static void main(String[] args) {
       Map<Integer,String> map = new LinkedHashMap<>();
       map.put(1, "one"); map.put(3, "three");
       map.put(2, "two"); map.put(5, "five");
       map.put(6, "six"); map.put(4,  "four");
       for(int key : map.keySet())
    	   System.out.print(key+" , ");
       // 1 , 3 , 2 , 5 , 6 , 4 ,
    }
}
```
## Removing a key from Map while iterating over the map, results in ConcurrentModificationException
```java
import java.util.*;

class Main {
   public static void main(String[] args) {
      Map<Integer, List<Integer>> map = new HashMap<>();
      map.put(9, Arrays.asList(1, 4));
      map.put(2, new ArrayList<Integer>());
      for (int key : map.keySet()) {
         if (map.get(key).size() == 0) {
            map.remove(key);
         }
      }
   }
}
```

```
Exception in thread "main" java.util.ConcurrentModificationException
	at java.base/java.util.HashMap$HashIterator.nextNode(HashMap.java:1606)
	at java.base/java.util.HashMap$KeyIterator.next(HashMap.java:1629)
	at Test/net.mahtabalam.Main.main(Main.java:15)
```

## To remove keys while iterating over map keys, use iterator.remove()
```java
import java.util.*;

class Main {
   public static void main(String[] args) {
      Map<Integer, List<Integer>> map = new HashMap<>();
      map.put(9, Arrays.asList(1, 4));
      map.put(2, new ArrayList<Integer>());
      Iterator<Integer> iterator =  map.keySet().iterator();
      while (iterator.hasNext()) {
    	 Integer key = iterator.next();
         if (map.get(key).size() == 0) {
            iterator.remove();
         }
      }
      System.out.println(map); // {9=[1, 4]}
   }
}
```


## ConcurrentHashMap : For higher throughput, doesn't lock entire map while performing a write

```
Read operations are guaranteed not to be blocked or block a key.
Write operations are blocked and block other writes at the map Entry level.
These two ideas are important in environments where we want to achieve high throughput and eventual consistency.

HashTable and Collections.synchronizedMap collections also implement concurrency for reads and writes.
However, they are less efficient because they lock the entire collection
instead of locking just the Entry at which the thread is writing.

On the other hand, the ConcurrentHashMap class locks at a map Entry level.
Thus, other threads are not blocked from writing on other map keys.
Therefore, to achieve high throughput, ConcurrentHashMap in multi-thread environments is a better option
when compared to HashTable and synchronizedMap collections.
```

## Collections.reverse() - works fine with list of lists 👍

```
list = [[1,2], [3], [4,5] , [6]]
Collections.reverse(list) 
// [[6], [4,5], [3], [1,2]]
``` 

## Convert an int to binary string
```
Integer.toBinaryString(2) => 10
Integer.toBinaryString(-2) => 11111111111111111111111111111110
```

## Bitwise OR(|) and AND(&) operation
```
int num5 = 5;
int num8 = 8;
int or = 5 | 8;
int and = 5 & 8;
System.out.println("5 | 8 =" + or);  // 13
System.out.println("5 & 8 =" + and); // 0
```

## String substring() method
```
String s = "a";
System.out.println(s.substring(1));   => "" (empty string)
System.out.println(s.substring(2));   => StringIndexOutOfBoundsException  
```

## Concat String and char
```
String s = "a" + "hello".charAt(1);
System.out.println(s); //ae
```

## StringBuilder apppend
```
StringBuilder sb = new StringBuilder();
sb.append('a');
sb.append(1);
sb.append("ABC");
System.out.println(sb); // a1ABC
```

## List add(int index, T element)
```
List<Integer> list = new ArrayList<>();
list.add(1); list.add(2); list.add(3);
list.add(1, 5);
System.out.println(list);  // [1, 5, 2, 3]
```

## List set(int index, T element)
```
List<Integer> list = new ArrayList<>();
list.add(1); list.add(2); list.add(3);
list.set(1, 5);
System.out.println(list);  // [1, 5, 3]
```

## Set contains()
```java

import java.util.*;
class Main {
  class Employee {
    int id;
    Employee(int id){
      this.id = id;
    }
  }
  public static void main(String[] args) {
    new Main().check();
  }

  private void check() {
    Employee e1 = new Employee(1);
    Employee e2 = new Employee(1);
    Set<Employee> set = new HashSet<>();
    set.add(e1);
    System.out.println(set.contains(e2)); // false
    set.add(e2);
    Employee e3 = e2;
    System.out.println(set.contains(e3)); // true
  }
}
```
