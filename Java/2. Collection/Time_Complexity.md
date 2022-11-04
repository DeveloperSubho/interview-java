T**ime Complexity**
Usually, when we talk about time complexity, we refer to Big-O notation. Simply put, the notation describes how the time to perform the algorithm grows with the input size.

**List**
Let's start with a simple list, which is an ordered collection.

Here we'll look at a performance overview of the ArrayList, LinkedList, and CopyOnWriteArrayList implementations.

**_3.1. ArrayList_**
The ArrayList in Java is backed by an array. This helps to understand the internal logic of its implementation. A more comprehensive guide for the ArrayList is available in this article.

So let's focus first on the time complexity of the common operations at a high level:

add() – takes O(1) time; however, worst-case scenario, when a new array has to be created and all the elements copied to it, it's O(n)
add(index, element) – on average runs in O(n) time
get() – is always a constant time O(1) operation
remove() – runs in linear O(n) time. We have to iterate the entire array to find the element qualifying for removal.
indexOf() – also runs in linear time. It iterates through the internal array and checks each element one by one, so the time complexity for this operation always requires O(n) time.
contains() – implementation is based on indexOf(), so it'll also run in O(n) time.

**3.2. CopyOnWriteArrayList**
This implementation of the List interface is beneficial when working with multi-threaded applications. It's thread-safe and explained well in this guide here.

Here's the Big-O notation performance overview for CopyOnWriteArrayList:
add() – depends on the position we add value, so the complexity is O(n)
get() – is O(1) constant time operation
remove() – takes O(n) time
contains() – likewise, the complexity is O(n)
As we can see, using this collection is very expensive because of the performance characteristics of the add() method.

**3.3. LinkedList**
LinkedList is a linear data structure that consists of nodes holding a data field and a reference to another node. For more LinkedList features and capabilities, have a look at this article here.

Let's present the average estimate of time we need to perform some basic operations:

add() – appends an element to the end of the list. It only updates a tail, and therefore, it's O(1) constant-time complexity.
add(index, element) – on average runs in O(n) time
get() – searching for an element takes O(n) time.
remove(element) – to remove an element, we first need to find it. This operation is O(n).
remove(index) – to remove an element by index, we first need to follow the links from the beginning; therefore, the overall complexity is O(n).
contains() – also has O(n) time complexity

4. Map
   With the latest JDK versions, we're witnessing significant performance improvement for Map implementations, such as replacing the LinkedList with the balanced tree node structure in HashMap, and LinkedHashMap internal implementations. This shortens the element lookup worst-case scenario from O(n) to O(log(n)) time during the HashMap collisions.

However, if we implement proper .equals() and .hashcode() methods, collisions are unlikely.

Storing and retrieving elements from the HashMap takes constant O(1) time.

**_4.2. Testing O(log(n)) Operations_**
For the tree structure TreeMap and ConcurrentSkipListMap, the put(), get(), remove(), and containsKey() operations time is O(log(n)).

**5. Set**
Generally, Set is a collection of unique elements. Here we're going to examine the HashSet, LinkedHashSet, EnumSet, TreeSet, CopyOnWriteArraySet, and ConcurrentSkipListSet implementations of the Set interface.

To better understand the internals of the HashSet, this guide is here to help.

Now let's jump ahead to present the time complexity numbers. For HashSet, LinkedHashSet, and EnumSet, the add(), remove() and contains() operations cost constant O(1) time thanks to the internal HashMap implementation.

Likewise, the TreeSet has O(log(n)) time complexity for the operations listed in the previous group. This is because of the TreeMap implementation. The time complexity for ConcurrentSkipListSet is also O(log(n)) time, as it's based in skip list data structure.

For CopyOnWriteArraySet, the add(), remove() and contains() methods have O(n) average time complexity.

![\Java-collection-complexity](/Screenshots/Java-collection-complexity.png)

List

LinkedList
Time

Linked lists have most of their benefit when it comes to the insertion and deletion of nodes in the list. Unlike the dynamic array, insertion and deletion at any part of the list takes constant time.

However, unlike dynamic arrays, accessing the data in these nodes takes linear time because of the need to search through the entire list via pointers. It’s also important to note that there is no way of optimizing search in linked lists. In the array, we could at least keep the array sorted. However, since we don’t know how long the linked list is, there is no way of performing a binary search

Insertion — O(1),Search — O(n).​Deletion — O(1),Indexing — O(n),

Space

Linked lists hold two main pieces of information (the value and pointer) per node. This means that the amount of data stored increases linearly with the number of nodes in the list. Therefore, the space complexity of the linked list is linear:

​Space — O(n)​.

ArrayList
Time

Inserting value at last or given index due to the added steps of creating a new array in ArrayList its worst-case complexity reaches to order of N, that is why we prefer using LinkedList where multiple inserts are required.

Searching : we have to iterate through all elements. This operation has O(N) time complexity.

Searching by indexing: ArrayList can give you any element in O(1) complexity as the array has random access property. You can access any index directly without iterating through the whole array.

Remove by value: To remove an element by value in ArrayList and LinkedList we need to iterate through each element to reach that index and then remove that value. This operation is of O(N) complexity.

Remove by index: To remove by index, ArrayList find that index using random access in O(1) complexity, but after removing the element, shifting the rest of the elements causes overall O(N) time complexity.

![\List-Time-Complexity](/Screenshots/List-Time-Complexity.png)

**Set**

HashSet, LinkedHashSet and TreeSet
time

Hashset is implemented using a hash table. elements are not ordered. the add, remove, and contains methods has constant time complexity o(1).

Treeset is implemented using a tree structure(red-black tree in algorithm book). the elements in a set are sorted, but the add, remove, and contains methods has time complexity of o(log (n)). it offers several methods to deal with the ordered set like first(), last(), headset(), tailset(), etc.

Linkedhashset is between hashset and treeset. it is implemented as a hash table with a linked list running through it, so it provides the order of insertion. the time complexity of basic methods is o(1).

![\Set-Time-Complexity](/Screenshots/Set-Time-Complexity.png)

**Queue**

Priority Deque

time

Java Priority Queue is implemented using Heap Data Structures and Heap has O(log(n)) time complexity to insert and delete element.
Offer() and add() methods are used to insert the element in the in the priority queue java program.
Poll() and remove() is used to delete the element from the queue.
Element retrieval methods i.e. peek() and element(), that are used to retrieve elements from the head of the queue is constant time i.e. O(1).
contains(Object)method that is used to check if a particular element is present in the queue, have leaner time complexity i.e. O(n).
Array Deque
time

In a doubly-linked list implementation and assuming no allocation/deallocation overhead, the time complexity of all deque operations is O(1). Additionally, the time complexity of insertion or deletion in the middle, given an iterator, is O(1); however, the time complexity of random access by index is O(n).

![\Queue-Time-Complexity](/Screenshots/Queue-Time-Complexity.png)

**Map**

LinkedHashMap
time

LinkedHashMap — uses a Hash table as its underlying data structure and preserves insertion order.

get() — retrieving takes O(1) constant-time
containsKey() — searching for an element takes O(1) time
next() — fetching an element takes O(1) time.
IdentityHashMap
IdentityHashMap — uses a Hash table

as its underlying data structure.

get() — retrieving takes O(1) constant-time
containsKey() — searching for an element takes O(1) time
next() — fetching an element takes O(h/n) time, where ‘h’ is table capacity.
TreeMap:
Java TreeMap class is a red-black tree based implementation. It provides an efficient means of storing key-value pairs in sorted order.

get() — retrieving takes O(log n) constant-time.
containsKey() — searching for an element takes O(log n) time.
next() — fetching each element takes O(log n) time.

![\Map-Time-Complexity](/Screenshots/Map-Time-Complexity.png)

Reference:
https://www.baeldung.com/java-collections-complexity
https://yogeshkkhichi.medium.com/time-and-space-complexity-of-collections-5a00c7b1d32b
