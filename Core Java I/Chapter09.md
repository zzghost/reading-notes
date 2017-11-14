# Collections
In the Java library, There are two fundamental interfaces for collections: *Collection* and *Map*. Collection interface has two fundamental methods:add() and iterator().  
Iterator interface has four methods: next(), hasNext(), remove(), forEachRemaining().  
Java8:`iterator.forEachRemaining(element -> do something with element)`  
e.g.: `list.forEach(s -> System.out.println(s));`

#### Concrete Collections in the Java library:
Collection Type| Description
---------------|------------
ArrayList|An indexed sequence that grows and shrinks dynamically
LinkedList|An ordered sequence that allows efficient insertion and removal at any location
ArrayDeque|A double-ended queue that is implemented as a circular array
HashSet|An unordered collection that rejects duplicates
TreeSet|A sorted set
EnumSet|A set of enumerated type values
LinkedHashSet|A set that remembers the order in which elements were inserted
PriorityQueue|A collection that allows efficient removal of the smallest element
HashMap|A data structure that stores key/value associations
TreeMap|A map in which the keys are sorted
EnumMap|A map in which the keys belong to an enumerated type
LinkedHashMap|A map that remembers the order in which entries were added
WeakHashMap|A map with values that can be reclaimed by the garbage collector if they are not used elsewhere
IdentityHashMap|A map with keys that are compared by ==, not equals.

**LinkedList**
##### java.util.list&lt;E&gt;
No.|method|Description
---|------|-----------
1 |ListIterator&lt;E&gt; listIterator()|returns a list iterator for visiting the elements of the list.
2 |ListIterator&lt;E&gt; listIterator(int index)|returns a list iterator for visiting the elements of the list whose first call to next will return the element with the given index.
3 |void add(int i, E element)|adds an element at *i*th position
4 |void addAll(int i, Collection&lt;? extends E&gt; elements)|adds all elements from a collection at *i*th position
5 |E remove(int i)|removes and returns the element at *i*th position
6 |E get(int i)|gets the element at *i*th position
7 |E set(int i, E element)|replaces the element at *i*th position with a new element and returns the old element
8 |int indexOf(Object element)|returns the position of the first occurrence of an element equal to the specified element, or -1 if no matching element is found
9 |int lastIndexOf(Object element)|returns the position of the last occurrence of an element equal to the specified element, or -1 if no matching element is found.
##### java.util.listIterator&lt;E&gt;
No.|method|Description
---|------|-----------
1. |void add(E element)|adds an element before the current position
2. |void set(E element)|replaces the last element visited by *next* or *previous* with a new element.Throws an *IllegalStateException* if the list structure was modified since the last call to *next* or *previous*
3. |boolean hasPrevious()|returns true if there's another element to visit when iterating backwards through the list.
4. |E previous()|returns the previous object.Throw a *NoSuchElementException* if the begining of the list has been reached.
5. |int nextIndex()|returns the index of the element that would be returned by the next call to *next*.
6. |int previousIndex()|returns the index of the element that would be returned by the next call to previous.
7. |LinkedList()|constructs an empty linked list.
8. |LinkedList(Collection&lt;? extends E&gt; elements)|constructs a linked list and adds all elements from a collection.
9. |void addFirst(E element)
10.|void addLast(E element)|adds an element to the begining or the end of the list
11.|E getFirst()
12.|E getLast()|returns the element at the begining or the end of the list
13.|E removeFirst()
14.|E removeLast()|removes and returns the element at the begining or the end of the list
All linked lists in java are actually *doubly linked*.   

**TreeSet**  
It's a sorted collection.The current implementation uses a *red-black tree*.  
Adding an element to a tree is slower than adding it to a hash table.But it is still much faster than checking for duplicates in an array or linked list.If the tree contains n elements, then an average of log2(n) comparisons are required to find the correct position for the new element.  

**Queues and Deques**  
##### java.util.Queue&lt;E&gt;
No.|method|Description
---|------|-----------
1. |boolean add(E element)
2. |boolean offer(E element)|adds the given element to the tail of this deque and returns true, provided the queue is not full.If the queue is full, the first method throws an *IllegalStateException*, whereas the second method returns *false*.
3. |E remove()
4. |E poll()|removes and returns the element at the head of this queue,provided that the queue is not empty.If the queue is empty, the first method throws a *NoSuchElementException*, whereas the second method returns *null*.
5. |E element()
6. |E peek()|returns the element at the head of this queue without removing it, provided the queue is not empty. If the queue is empty, the first method throws a *NoSuchElementException*, whereas the second method returns *null*.
##### java.util.Deque&lt;E&gt;
No.|method|Description
---|------|-----------
1. |void addFirst(E element)
2. |void addLast(E element)
3. |boolean offerFirst(E element)
4. |boolean offerLast(E element)|adds the given element to the head or tail of this deque. If the queue is full, the first two methods throws an *IllegalStateException*, whereas the last two methods return *false*.
5. |E removeFirst()
6. |E removeLast()
7. |E pollFirst()
8. |E pollLast()| removes and returns the element at the head of this queue, provided the queue is not empty. If the queue is empty, the first two methods throws a *NoSuchElementException*, whereas the last two methods return *null*.
9. |E getFirst()
10.|E getLast()
11.|E peekFirst()
12.|E peekLast()|returns the element at the head of this queue without removing it, provided the queue is not empty.If the queue is empty, the first two methods throw a *NoSuchElementException*, whereas the last two methods return *null*.
##### java.util.ArrayDeque&lt;E&gt;
No.|method|Description
---|------|-----------
1. |ArrayDeque()
2. |ArrayDeque(int initialCapacity)|constructs an unbounded deque with an inital capacity of 16 or the given inital capacity
##### java.util.PriorityQueue
No.|method|Description
---|------|-----------
1. |PriorityQueue()
2. |PriorityQueue(int initialCapacity)|constructs a priority queue for sorting Comparable objects.
3. |PriorityQueue(int initialCapacity, Comparator&lt;? super E &gt; c)|constructs a priority queue and uses the specified comparator for sorting its elements.

**Maps**
##### java.util.Map&lt;K, V&gt;
No.|method|Description
---|------|-----------
1. |V get(Object key)|gets the value associated with the key;returns the object associated with the key, or null if the key is not found in the map.Implementing classes may forbid null keys.
2. |default V getOrDefault(Object key, V defaultValue)|gets the value associated with the key; returns the object associated with the key, or defaultValue if the key is not found in the map
3. |V put(K key, V value)|puts the association of a key and a value into the map. If the key is already present, the new object replaces the old one previously associated with the key.This method returns the old value of the key, or null if the key was not previously present.Implementing classes may forbid null keys or values.
4. |void putAll(Map&lt;? extends K, ? extends V&gt; entries)|adds all entries from the specified map to this map
5. |boolean containsKey(Object key)|return true if the key is present in the map
6. |boolean containsValue(Object value)|returns true if the value is present in the map
7. |default void forEach(BiConsumer&lt;? super K, ? super V&gt; action)|Applies the action to all key/value pairs of this map.
##### Update map entries,java.util.Map&lt;K, V&gt;:（new in Java8）
No.|method|Description
---|------|-----------
1. |default V merge(K key, V value, BiFunction&lt;? super V, ? super V, ? extends V&gt; remappingFunction)|If key is associated with a non-null value *v*, applies the function to *v* and *value* and either associates *key* with the result or, if the result is null, removes the *key*.Otherwise, associates *key* with *value*.Returns get(key).
2. |default V compute(K key, BiFunction&lt;? super K, ? super V, ? extends V) remappingFunction)|Applies the function to *key* and get(key).Either associates *key* with the result or, if the result is *null*, removes the key.Returns get(key)
3. |default V computeIfPresent(K key, BiFunction&lt;? super K, ? super V, ? extends V) remappingFunction)|If *key* is associated with a non-null value *v*, applies the function to *key* and *v* and either associated *key* with the result or, if the result is null, removes the key.Returns get(key)
4. |default V computeIfAbsent(K key, BiFunction&lt;? super K, ? super V, ? extends V) remappingFunction)|Applies the function to *key* unless *key* is associated with a non-null value. Either associated *key* with the result or, if the result is null, removes the key.Returns get(key)
5. |default void replaceAll(BiFunction&lt;? super K, ? super V, ? extends V) function)|Calls the function on all entries.associates keys with non-null results and removes the keys with *null* results.
##### Map views, java.util.Map&lt;K, V&gt;:
No.|method|Description
---|------|-----------
1. |Set&lt;Map.Entry&lt;K, V&gt;&gt; entrySet()|returns a set view of Map.Entry objects, the key/value pairs in the map.You can remove elements from this set and they are removed from the map, but you cannot add any elements.
2. |Set&lt;K&gt; keySet()|returns a set view of all keys in the map.You can remove elements from this set and the keys and associated values are removed from the map, but you cannot add any elements
3. |Collection&lt;V&gt; values()|returns a collection view of all values in the map. You can remove elements from this set and the removed value and its key are removed from the map, but you cannot add any elements.
##### java.util.Map.Entry&lt;K, V&gt;
No.|method|Description
---|------|-----------
1. |K getKey()
2. |K getValue()|returns the key or value of this entry.
3. |K setValue(V newValue)|changes the value in the associated map to the new value and returns the old value.
##### java.util.HashMap&lt;K, V&gt;
No.|method|Description
---|------|-----------
1. |HashMap()
2. |HashMap(int initialCapacity)
3. |HashMap(int initialCapacity, float loadFactor)|constructs an empty hash map with the specified capacity and load factor(a number between 0.0 and 1.0 that determines at what percentage of fullness the hash table will be rehashed into a larger one).The default load factor is 0.75
##### java.util.TreeMap&lt;K, V&gt;
No.|method|Description
---|------|-----------
1. |TreeMap()|constructs an empty tree map for keys that implement the Comparable interface
2. |TreeMap(Comparator&lt;? super K&gt; c)constructs a tree map and uses the specified comparator for sorting its keys.
3. |TreeMap(Map&lt;? extends K, ? extends V&gt; entries)|constructs a tree map and adds all entries from a map
4. |TreeMap(SortedMap&lt;? extends K, ? extends V&gt; entries)|constructs a tree map, adds all entries from a sorted map, and uses the same element comparator as the given sorted map.
##### java.util.SortedMap&lt;K, V&gt;
No.|method|Description
---|------|-----------
1. |Comparator&lt;? super K&gt; comparator()|returns the comparator used for sorting the keys, or null if the keys are compared with the compareTo method of the Comparable interface
2. |K firstKey()
3. |K lastKey()|returns the smallest or largest key in the map

**Map views**  
You can obtain *views* of the map, objects that implement the *Collection* interface or one of its subinterfaces.    
There are three views:  
  + Set&lt;K&gt; keySet()
  + Collection&lt;V&gt; values()
  + Set&lt;Map.Entry&lt;K, V&gt;&gt; entrySet()
keySet() is not a HashSet or TreeSet, it is an object of some other class that implements the Set interface.
