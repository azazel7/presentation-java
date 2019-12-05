# How to iterate over a collection
A quick presentation of collection browsing.

## Previously
- A collection is an interface to store an ensemble of objects.
- Many implementations for Collections.
- We saw the `LinkedList`.
- We used it.

```java
LinkedList<Integer> otter = new LinkedList<Integer>();
for(int i = 0; i < 10; ++i)
	otter.add(i);
for(int i = 0; i < otter.size(); ++i)
	System.out.println(otter.get(i));
```

## The issues
- `LinkedList` are not always the best choice.
- Other collections cannot be accessed like the `LinkedList`.

## Exercice 1
### Goal
Print the content of each element in `data`.

```java
public class Main {
	
	public static Collection<String> loadData(String filename) throws IOException{
		Collection<String> ret = new LinkedList<String>();
		BufferedReader csvReader = new BufferedReader(new FileReader(filename));
		String row;
		int line_count = 0;
		while ((row = csvReader.readLine()) != null) {
			if(line_count > 0) {
				String[] data = row.split(",");
				ret.add(row);
			}
		    line_count += 1;
		}
		csvReader.close();
		return ret;
	}
	public static void main(String[] args) {
		Collection<String> data;
		try {
			data = loadData("/home/magoa/phd/TA/LA1/data/frenepublicinjection2014.csv");
		} catch (IOException e1) {
			e1.printStackTrace();
		}
		//Write the code here
	}

}
```
### Tips
- `for` loop does not work.
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
## Exercice 2
### Goal
Remove the element that contains a quote (").

```java
public class Main {
	
	public static Collection<String> loadData(String filename) throws IOException{
		Collection<String> ret = new LinkedList<String>();
		BufferedReader csvReader = new BufferedReader(new FileReader(filename));
		String row;
		int line_count = 0;
		while ((row = csvReader.readLine()) != null) {
			if(line_count > 0) {
				String[] data = row.split(",");
				ret.add(row);
			}
		    line_count += 1;
		}
		csvReader.close();
		return ret;
	}
	public static void main(String[] args) {
		Collection<String> data;
		try {
			data = loadData("/home/magoa/phd/TA/LA1/data/frenepublicinjection2014.csv");
		} catch (IOException e1) {
			e1.printStackTrace();
			//Write the code here
		}
	}

}
```
### Tips
- `forEach` does not work.
- `Iterator` are the key.
	- `hasNext()`
	- `next()`

## Exercice 2
Remove the element that contains a quote (").

```java
public class Main {
	
	public static Collection<String> loadData(String filename) throws IOException{
		Collection<String> ret = new LinkedList<String>();
		BufferedReader csvReader = new BufferedReader(new FileReader(filename));
		String row;
		int line_count = 0;
		while ((row = csvReader.readLine()) != null) {
			if(line_count > 0) {
				String[] data = row.split(",");
				ret.add(row);
			}
		    line_count += 1;
		}
		csvReader.close();
		return ret;
	}
	public static void main(String[] args) {
		Collection<String> data;
		try {
			data = loadData("/home/magoa/phd/TA/LA1/data/frenepublicinjection2014.csv");
		} catch (IOException e1) {
			e1.printStackTrace();
			//Write the code here
		}
	}

}
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
