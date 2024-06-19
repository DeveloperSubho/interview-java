**Hashmap:**

Let's first look at what it means that HashMap is a map. A map is a key-value mapping, which means that every key is mapped to exactly one value and that we can use the key to retrieve the corresponding value from a map.

One might ask why not simply add the value to a list. Why do we need a HashMap? The simple reason is performance. If we want to find a specific element in a list, the time complexity is O(n) and if the list is sorted, it will be O(log n) using, for example, a binary search.

The advantage of a HashMap is that the time complexity to insert and retrieve a value is O(1) on average. We'll look at how that can be achieved later. Let's first look at how to use HashMap.

**_Setup:_**
Let's create a simple class that we'll use throughout the article:

```
public class Product {

    private String name;
    private String description;
    private List<String> tags;

    // standard getters/setters/constructors

    public Product addTagsOfOtherProduct(Product product) {
        this.tags.addAll(product.getTags());
        return this;
    }
}
```

**_Put_**
We can now create a HashMap with the key of type String and elements of type Product:

```
Map<String, Product> productsByName = new HashMap<>();
```

And add products to our HashMap:

```
    Product eBike = new Product("E-Bike", "A bike with a battery");
    Product roadBike = new Product("Road bike", "A bike for competition");
    productsByName.put(eBike.getName(), eBike);
    productsByName.put(roadBike.getName(), roadBike);
```

**_Get_**
We can retrieve a value from the map by its key:

```
    Product nextPurchase = productsByName.get("E-Bike");
    assertEquals("A bike with a battery", nextPurchase.getDescription());
```

If we try to find a value for a key that doesn't exist in the map, we'll get a null value:

```
    Product nextPurchase = productsByName.get("Car");
    assertNull(nextPurchase);
```

And if we insert a second value with the same key, we'll only get the last inserted value for that key:

```
    Product newEBike = new Product("E-Bike", "A bike with a better battery");
    productsByName.put(newEBike.getName(), newEBike);
    assertEquals("A bike with a better battery", productsByName.get("E-Bike").getDescription());
```

**_Null as the Key_**
HashMap also allows us to have null as a key:

```
    Product defaultProduct = new Product("Chocolate", "At least buy chocolate");
    productsByName.put(null, defaultProduct);

    Product nextPurchase = productsByName.get(null);
    assertEquals("At least buy chocolate", nextPurchase.getDescription());

```

Values with the Same Key
Furthermore, we can insert the same object twice with a different key:

productsByName.put(defaultProduct.getName(), defaultProduct);
assertSame(productsByName.get(null), productsByName.get("Chocolate"));
Copy
2.6. Remove a Value
We can remove a key-value mapping from the HashMap:

productsByName.remove("E-Bike");
assertNull(productsByName.get("E-Bike"));
Copy
2.7. Check If a Key or Value Exists in the Map
To check if a key is present in the map, we can use the containsKey() method:

productsByName.containsKey("E-Bike");
Copy
Or, to check if a value is present in the map, we can use the containsValue() method:

productsByName.containsValue(eBike);
Copy
Both method calls will return true in our example. Though they look very similar, there is an important difference in performance between these two method calls. The complexity to check if a key exists is O(1), while the complexity to check for an element is O(n), as it's necessary to loop over all the elements in the map.

2.8. Iterating Over a HashMap
There are three basic ways to iterate over all key-value pairs in a HashMap.

We can iterate over the set of all keys:

for(String key : productsByName.keySet()) {
Product product = productsByName.get(key);
}
Copy
Or we can iterate over the set of all entries:

for(Map.Entry<String, Product> entry : productsByName.entrySet()) {
Product product = entry.getValue();
String key = entry.getKey();
//do something with the key and value
}
Copy
Fiinally, we can iterate over all values:

List<Product> products = new ArrayList<>(productsByName.values());
Copy 3. The Key
We can use any class as the key in our HashMap. However, for the map to work properly, we need to provide an implementation for equals() and hashCode(). Let's say we want to have a map with the product as the key and the price as the value:

HashMap<Product, Integer> priceByProduct = new HashMap<>();
priceByProduct.put(eBike, 900);
Copy
Let's implement the equals() and hashCode() methods:

@Override
public boolean equals(Object o) {
if (this == o) {
return true;
}
if (o == null || getClass() != o.getClass()) {
return false;
}

    Product product = (Product) o;
    return Objects.equals(name, product.name) &&
      Objects.equals(description, product.description);

}

@Override
public int hashCode() {
return Objects.hash(name, description);
}
Copy
Note that hashCode() and equals() need to be overridden only for classes that we want to use as map keys, not for classes that are only used as values in a map. We'll see why this is necessary in section 5 of this article.

4. Additional Methods as of Java 8
   Java 8 added several functional-style methods to HashMap. In this section, we'll look at some of these methods.

For each method, we'll look at two examples. The first example shows how to use the new method, and the second example shows how to achieve the same in earlier versions of Java.

As these methods are quite straightforward, we won't look at more detailed examples.

4.1. forEach()
The forEach method is the functional-style way to iterate over all elements in the map:

productsByName.forEach( (key, product) -> {
System.out.println("Key: " + key + " Product:" + product.getDescription());
//do something with the key and value
});
Copy
Prior to Java 8:

for(Map.Entry<String, Product> entry : productsByName.entrySet()) {
Product product = entry.getValue();
String key = entry.getKey();
//do something with the key and value
}
Copy
Our article Guide to the Java 8 forEach covers the forEach loop in greater detail.

4.2. getOrDefault()
Using the getOrDefault() method, we can get a value from the map or return a default element in case there is no mapping for the given key:
Product chocolate = new Product("chocolate", "something sweet");
Product defaultProduct = productsByName.getOrDefault("horse carriage", chocolate);
Product bike = productsByName.getOrDefault("E-Bike", chocolate);
Copy
Prior to Java 8:

Product bike2 = productsByName.containsKey("E-Bike")
? productsByName.get("E-Bike")
: chocolate;
Product defaultProduct2 = productsByName.containsKey("horse carriage")
? productsByName.get("horse carriage")
: chocolate;
Copy
4.3. putIfAbsent()
With this method, we can add a new mapping, but only if there is not yet a mapping for the given key:

productsByName.putIfAbsent("E-Bike", chocolate);
Copy
Prior to Java 8:

if(productsByName.containsKey("E-Bike")) {
productsByName.put("E-Bike", chocolate);
}
Copy
Our article Merging Two Maps with Java 8 takes a closer look at this method.

4.4. merge()
And with merge(), we can modify the value for a given key if a mapping exists, or add a new value otherwise:

Product eBike2 = new Product("E-Bike", "A bike with a battery");
eBike2.getTags().add("sport");
productsByName.merge("E-Bike", eBike2, Product::addTagsOfOtherProduct);
Copy
Prior to Java 8:

if(productsByName.containsKey("E-Bike")) {
productsByName.get("E-Bike").addTagsOfOtherProduct(eBike2);
} else {
productsByName.put("E-Bike", eBike2);
}
Copy
4.5. compute()
With the compute() method, we can compute the value for a given key:

productsByName.compute("E-Bike", (k,v) -> {
if(v != null) {
return v.addTagsOfOtherProduct(eBike2);
} else {
return eBike2;
}
});
Copy
Prior to Java 8:
if(productsByName.containsKey("E-Bike")) {  
 productsByName.get("E-Bike").addTagsOfOtherProduct(eBike2);
} else {
productsByName.put("E-Bike", eBike2);
}
Copy
It's worth noting that the methods merge() and compute() are quite similar. The compute() method accepts two arguments: the key and a BiFunction for the remapping. And merge() accepts three parameters: the key, a default value to add to the map if the key doesn't exist yet, and a BiFunction for the remapping.

5. HashMap Internals
   In this section, we'll look at how HashMap works internally and what are the benefits of using HashMap instead of a simple list, for example.

As we've seen, we can retrieve an element from a HashMap by its key. One approach would be to use a list, iterate over all elements, and return when we find an element for which the key matches. Both the time and space complexity of this approach would be O(n).

With HashMap, we can achieve an average time complexity of O(1) for the put and get operations and space complexity of O(n). Let's see how that works.

5.1. The Hash Code and Equals
Instead of iterating over all its elements, HashMap attempts to calculate the position of a value based on its key.

The naive approach would be to have a list that can contain as many elements as there are keys possible. As an example, let's say our key is a lower-case character. Then it's sufficient to have a list of size 26, and if we want to access the element with key ‘c', we'd know that it's the one at position 3, and we can retrieve it directly.

However, this approach would not be very effective if we have a much bigger keyspace. For example, let's say our key was an integer. In this case, the size of the list would have to be 2,147,483,647. In most cases, we would also have far fewer elements, so a big part of the allocated memory would remain unused.

HashMap stores elements in so-called buckets and the number of buckets is called capacity.
When we put a value in the map, the key's hashCode() method is used to determine the bucket in which the value will be stored.

To retrieve the value, HashMap calculates the bucket in the same way – using hashCode(). Then it iterates through the objects found in that bucket and use key's equals() method to find the exact match.

5.2. Keys' Immutability
In most cases, we should use immutable keys. Or at least, we must be aware of the consequences of using mutable keys.

Let's see what happens when our key changes after we used it to store a value in a map.

For this example, we'll create the MutableKey:

public class MutableKey {
private String name;

    // standard constructor, getter and setter

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        MutableKey that = (MutableKey) o;
        return Objects.equals(name, that.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }

}
Copy
And here goes the test:

MutableKey key = new MutableKey("initial");

Map<MutableKey, String> items = new HashMap<>();
items.put(key, "success");

key.setName("changed");

assertNull(items.get(key));
Copy
As we can see, we're no longer able to get the corresponding value once the key has changed, instead, null is returned. This is because HashMap is searching in the wrong bucket.

The above test case may be surprising if we don't have a good understanding of how HashMap works internally.
5.3. Collisions
For this to work correctly, equal keys must have the same hash, however, different keys can have the same hash. If two different keys have the same hash, the two values belonging to them will be stored in the same bucket. Inside a bucket, values are stored in a list and retrieved by looping over all elements. The cost of this is O(n).

As of Java 8 (see JEP 180), the data structure in which the values inside one bucket are stored is changed from a list to a balanced tree if a bucket contains 8 or more values, and it's changed back to a list if, at some point, only 6 values are left in the bucket. This improves the performance to be O(log n).

5.4. Capacity and Load Factor
To avoid having many buckets with multiple values, the capacity is doubled if 75% (the load factor) of the buckets become non-empty. The default value for the load factor is 75%, and the default initial capacity is 16. Both can be set in the constructor.

5.5. Summary of put and get Operations
Let's summarize how the put and get operations work.

When we add an element to the map, HashMap calculates the bucket. If the bucket already contains a value, the value is added to the list (or tree) belonging to that bucket. If the load factor becomes bigger than the maximum load factor of the map, the capacity is doubled.

When we want to get a value from the map, HashMap calculates the bucket and gets the value with the same key from the list (or tree).

6. Conclusion
   In this article, we saw how to use a HashMap and how it works internally. Along with ArrayList, HashMap is one of the most frequently used data structures in Java, so it's very handy to have good knowledge of how to use it and how it works under the hood. Our article The Java HashMap Under the Hood covers the internals of HashMap in more detail.

Reference: https://www.baeldung.com/java-hashmap
