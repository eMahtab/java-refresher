# Java Refresher

**ArrayDeque Vs. LinkedList**

`java.util.ArrayDeque` doesn't allow null while `java.util.LinkedList` does allow adding null.

**Convert an int to binary string**
```
Integer.toBinaryString(2) => 10
Integer.toBinaryString(-2) => 11111111111111111111111111111110

```

**String substring() method**
```
String s = "a";
System.out.println(s.substring(1));   => "" (empty string)
System.out.println(s.substring(2));   => StringIndexOutOfBoundsException  
```

**Concat String and char**
```
String s = "a" + "hello".charAt(1);
System.out.println(s); //ae
```

**List add(int index, T element)**
```
List<Integer> list = new ArrayList<>();
list.add(1); list.add(2); list.add(3);
list.add(1, 5);
System.out.println(list);  // [1, 5, 2, 3]
```

**List set(int index, T element)**
```
List<Integer> list = new ArrayList<>();
list.add(1); list.add(2); list.add(3);
list.set(1, 5);
System.out.println(list);  // [1, 5, 3]
```
