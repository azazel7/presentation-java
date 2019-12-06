# How to iterate over a collection
A quick presentation of collection browsing.

## Previously
- A collection is an interface to store an ensemble of objects.
- Many implementations for Collections.
- We saw the `LinkedList`.
- We used it and we accessed the elements.

```java
LinkedList<Integer> otter = new LinkedList<Integer>();
for(int i = 0; i < 10; ++i)
	otter.add(i);
for(int i = 0; i < otter.size(); ++i)
	System.out.println(otter.get(i));
```

## The issues
- Accessing a `LinkedList` like that is inefficient.
- `LinkedList` are not always the best choice.
- Other collections cannot be accessed like the `LinkedList`.

## Exercice 1
### Goal
How many element in `data` start with the letter 'V'.

```java
import java.util.*;
import java.io.*;

public class Main {
	//The function to load the data
	public static Collection<String> loadData(String filename) throws IOException{
		Collection<String> ret = new LinkedList<String>();
		BufferedReader csvReader = new BufferedReader(new FileReader(filename));
		String row;
		int line_count = 0;
		while ((row = csvReader.readLine()) != null) {
			if(line_count > 0) {
				ret.add(row);
			}
		    line_count += 1;
		}
		csvReader.close();
		return ret;
	}
	public static void main(String[] args) throws IOException {
		Collection<String> data;
		data = loadData("data.txt");
		//Write your code here	
	}
}
```
### Tips
- `for` loop does not work ([documentation][https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html].
- `forEach` may be interesting.
```java
Collection<Integer> otter = new LinkedList<Integer>();
for(int i = 0; i < 10; ++i)
	otter.add(i);

//For each loop with a lambda function
otter.forEach(e -> System.out.println(e));

//For each loop with the keyword for
for(Integer e : otter){
	System.out.println(e);
}
```
- `charAt` to access a char in a String.

## Exercice 2
### Goal
Remove the element that contains a quote (").

### Tips
- `forEach` does not work.
- `Iterator` is the key.
	- `hasNext()` to check if there is more item.
	- `next()` to move to the next item and access it.
	- `remove` to remove an item.
```java
Collection<Integer> ducks = new HashSet<Integer>();
for(int i = 0; i < 10; ++i)
	ducks.add(i);
Iterator<Integer> it = ducks.iterator();

//While there is element in the iterator, we keep iterating
while(it.hasNext()) {
	Integer current = it.next(); //Access the next element
	System.out.println(current);

	//If current is an even number, remove it.
	if(current % 2 == 0)
		it.remove();
}
```

## Exercice 3
### Goal
Do a complete histogramm of the first letter of each element ... but two time faster!

### Tips
- `Spliterator` are splittable iterator.
	- `tryAdvance` try to move the iterator while applying a function.
	- `forEachRemaining` apply a function to all the remaining elements.
```java
import java.util.*;
import java.io.*;

public class BasicInteger {
	Integer a = 0;
	public BasicInteger() {
		a = 0;
	}
	public BasicInteger(Integer start) {
		a = start;
	}
	public void print() {
		System.out.println(a);
	}
	public void inc() {
		a += 1;
	}
	public void inc(int b) {
		a += b;
	}
	public void dec() {
		a -= 1;
	}
	public void dec(int b) {
		a -= b;
	}
}
public class Main {
	public static void inc(Spliterator<BasicInteger> c) {
		c.forEachRemaining(e -> e.inc(10));
	}
	public static void dec(Spliterator<BasicInteger> c) {
		c.forEachRemaining(e -> e.dec(10));
	}
	public static void main(String[] args) throws IOException {
		Collection<BasicInteger> ducks = new HashSet<BasicInteger>();
		for(int i = 0; i < 10; ++i)
			ducks.add(new BasicInteger(i));
		
		Spliterator<BasicInteger> sp1 = ducks.spliterator();
		Spliterator<BasicInteger> sp2 = sp1.trySplit();
		System.out.println(sp1.estimateSize());
		System.out.println(sp2.estimateSize());
		System.out.println("Splity");
		inc(sp1);
		dec(sp2);
		ducks.forEach(e -> e.print());
	}
}
```
- `Thread` are object to run multiple functions at once.
```java
//Create a task object which will be the function executed by the thread
Runnable task = () -> {
	String threadName = Thread.currentThread().getName();
	System.out.println("Hello " + threadName);
};

//Run the task in the main thread.
task.run();

//Create a new Thread object, then start it
Thread thread = new Thread(task);
thread.start();
```

## Conclusion
### forEach loop
- Straightforward.
- Less control on the flow.
- Element are mutable.
- No replacement.
- Mono thread.
- Cannot change the list.
- No dependencies between elements.

### Iterators
- Iterate step by step.
- Remove element.
- Stop at any point.
- Mono thread.

### Spliterator
- Split the iterator in multiple iterator.
- Easier to parallelize.
- Own similar function like the previous ones (`forEachRemaining` and `tryAdvance`).
- Split depends on implementation.
- Cannot change the list.

### Stream
- View of the data.
- Lazy evaluation.
- MapReduce API (Big Data).
- Parallel implementation.
- No dependencies between elements.

## Useful links
- [Stream API][https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html]
- [Iterator API][https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html]
- [Spliterator API][https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html]
- [Data file][http://donnees.ville.montreal.qc.ca/dataset/ebb813dd-a93f-4fb0-8137-80492a30a1fa/resource/0a5984e4-752f-401e-b2d9-aa0567535d39/download/frenepublicinjection2016.csv]
