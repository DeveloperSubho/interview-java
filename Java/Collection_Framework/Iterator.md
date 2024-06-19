The Iterator interface is implemented by collection classes that intend to provide a uniform way of accessing their items sequentially.

```
public interface Iterator<E> {
boolean hasNext();

    E next();

    void remove();

}
```

The Iterable interface on the other hand is implemented by classes that can be the target of the foreach statement or in other words can produce an iterator. The foreach construct was introduced in Java 5 and allows for more concise code. The interface definition is given below:

```
public interface Iterable<T> {

    Iterator<T> iterator();

}
```
