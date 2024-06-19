1. Concurrent collections vs synchronized collections:

2. What is the difference between a LinkedHashSet and a HashSet?

The difference between the two classes is the order in which the elements of the two collections can be iterated upon.

LinkedHashSet uses a linked list to maintain the order of insertion of key, value pairs and an iterator of the class can iterate over the (key, value) pairs in their inserted order. On the contrary, the HashSet's iterator will iterate over the inserted (key, value) pairs in an arbitrary order.

3. What is the difference between Hashtable and HashMap?

The differences are:

Hashtable is thread-safe and predates HashMap. On the other hand HashMap promises no thread safety.

Hashtable doesn't allow null keys, whereas HashMap allows a single null key.

HashMaphas a subclass LinkedHashMap which can be used to iterate over entries in the insertion order, whereas there's no such facility for the Hashtable.

Hashtable is supported but considered obsolete. If a thread-safe version of the HashMap is required, we can always use static methods to get synchronized wrappers using Collections.synchronizedMap() or use ConcurrentHashMap .
