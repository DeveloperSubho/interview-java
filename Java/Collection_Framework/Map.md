A map contains values on the basis of key, i.e. key and value pair. Each key and value pair is known as an entry. A Map contains unique keys.

**Java Map Hierarchy**
There are two interfaces for implementing Map in java: Map and SortedMap, and three classes: HashMap, LinkedHashMap, and TreeMap. The hierarchy of Java Map is given below:
![Java Map Hierarchy](../../Screenshots/java-map-hierarchy.png)

A Map doesn't allow duplicate keys, but you can have duplicate values. HashMap and LinkedHashMap allow null keys and values, but TreeMap doesn't allow any null key or value.

A Map can't be traversed, so you need to convert it into Set using keySet() or entrySet() method.

| Class         | Description                                                                                          |
| ------------- | ---------------------------------------------------------------------------------------------------- |
| HashMap       | HashMap is the implementation of Map, but it doesn't maintain any order.                             |
| LinkedHashMap | LinkedHashMap is the implementation of Map. It inherits HashMap class. It maintains insertion order. |
| TreeMap       | TreeMap is the implementation of Map and SortedMap. It maintains ascending order.                    |

```
Map<Integer,String> map=new HashMap<Integer,String>();
```

**_HashMap_**
Java HashMap class hierarchy
Java HashMap class implements the Map interface which allows us to store key and value pair, where keys should be unique. If you try to insert the duplicate key, it will replace the element of the corresponding key. It is easy to perform operations using the key index like updation, deletion, etc. HashMap class is found in the java.util package.

HashMap in Java is like the legacy Hashtable class, but it is not synchronized. It allows us to store the null elements as well, but there should be only one null key. Since Java 5, it is denoted as HashMap<K,V>, where K stands for key and V for value. It inherits the AbstractMap class and implements the Map interface.

```
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable
```

**_LinkedHashMap_**
LinkedHashMap class is Hashtable and Linked list implementation of the Map interface, with predictable iteration order. It inherits HashMap class and implements the Map interface.

```
public class LinkedHashMap<K,V> extends HashMap<K,V> implements Map<K,V>
```

**_TreeMap_**
TreeMap class is a red-black tree based implementation. It provides an efficient means of storing key-value pairs in sorted order.

```
public class TreeMap<K,V> extends AbstractMap<K,V> implements NavigableMap<K,V>, Cloneable, Serializable
```

**_Hashtable_**
Hashtable class implements a hashtable, which maps keys to values. It inherits Dictionary class and implements the Map interface.

```
public class Hashtable<K,V> extends Dictionary<K,V> implements Map<K,V>, Cloneable, Serializable
```

| HashMap                                                                                                    | Hashtable                                                                                           |
| ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| HashMap is non synchronized.                                                                               | It is not-thread safe and can't be shared between many threads without proper synchronization code. | Hashtable is synchronized. It is thread-safe and can be shared with many threads. |
| HashMap allows one null key and multiple null values.                                                      | Hashtable doesn't allow any null key or value.                                                      |
| HashMap is a new class introduced in JDK 1.2.                                                              | Hashtable is a legacy class.                                                                        |
| HashMap is fast.                                                                                           | Hashtable is slow.                                                                                  |
| We can make the HashMap as synchronized by calling this code Map m = Collections.synchronizedMap(hashMap); | Hashtable is internally synchronized and can't be unsynchronized.                                   |
| HashMap is traversed by Iterator.                                                                          | Hashtable is traversed by Enumerator and Iterator.                                                  |
| Iterator in HashMap is fail-fast.                                                                          | Enumerator in Hashtable is not fail-fast.                                                           |
| HashMap inherits AbstractMap class.                                                                        | Hashtable inherits Dictionary class.                                                                |

_References:_
https://www.javatpoint.com/java-map
