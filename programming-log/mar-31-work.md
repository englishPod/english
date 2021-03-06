#Java lab 8 homework
* There is Collection copy method addAll()/putAll() to copy one Collection/Map to the new Collection/Map by values, otherwise, it is just transmitting reference !

* The inner class MultiHashMapEntry does not need to override `equals()` and `hashCode()`, the reason is `MultiHashMap` extends `Object` class implicitly, and in client code which not involves any `equals()` or `hashCode()` calculation, so no need to override that two methods.

* for generic type, cause generic parameters are all erased in runtime, so the following code all prints true:
```java
Collection<Integer> arrayColl = new ArrayList<>();
if(arrayColl instanceof Collection);
if(arrayColl instanceof ArrayList);
```

* For Collection and Map interfaces, all put,add,construction methods store reference, which means any modification on referenced object also change the state of for example, ArrayList.
    * Primitive types are immutable, the only way to change value in an index is by remove() then set();

* For generic type, distinguish with generic type of Class, and generic type of Interface. For example:
```java
Map<K, Collection<V>> delegate = new TreeMap<>();
ArrayList<V> valueCollection = new ArrayList<>();
valueCollection.add(someValue);
delegate.put(someKey, valueCollection);

```
It is OK method, because ArrayList implements Collection. If they are inheritance relationship, then can't because in this way, the wildcard must be used.

## FAQ part
* Question: What are the reasons behind the dcision to not have a fully generic get method in the interface of `java.util.Map<K,V>`

> clarify the question, the signature of the method is `V get(Object key)` instead of `V get(K key)`

Answer: the reason why `get()`, etc, is not generic because the key of the entry you are retrieving does not have to be the same type as the object that you pass in to `get()`; the specification of the method only requires that they be equal. This follows from how the `equals()` method takes in an Object as parameter, not just the same type as the object.

Although it may be commonly true that many classes have `equals()`  defined so that its objects can only be equal to objects of its own class, there are many places in Java where this is not the case. For example, the specification for `List.equals()` says that two List objects are equal if they are both Lists and have the same contents, even if they are different implementation of `List`. So coming back to the example in this question, according to the specification of the method is possible to have a `Map<ArrayList, Something` and for me to call `get()` with a `LinkedList` as argument, and it should retrieve the key which is a list with the same contents. This would not be possible if `get()` were generic and restricted its argument type.


* Question: What is construction for generic class?
> clarify the question:

```Java
public Multimap<>{}
```

or

```Java
public Multimap(){}
```
