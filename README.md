# studio-test-ocp-java11
[Array Methods](https://github.com/fagarinis/studio-test-ocp-java11/Array-methods)

### Abstract class
Abstract methods in an abstract class cannot be private 

### Annotation
Retention policy: determina quando le annotazioni devono essere scartate

Tipi di retention policy: 

* SOURCE: will be retained only with source code, and discarded during compile time. 
Such annotations are used with build-tools that scan the file.
* CLASS: will be retained till compiling the code (inserted in .class file), and discarded during runtime (DEFAULT).
* RUNTIME: will be available to the JVM through runtime. 
this annotation should be available via reflection at runtime.

#### Marker annotation

A marker annotation is one that contains no elements, optional or required. example: @SafeVarargs, @Override, @Documented, and @FunctionalInterface 



### Array methods

#### COMPARE
compare methods of Arrays class compare the arrays lexicographically and return below values:

* 0:  if the first and second array are equal and contain the same elements in the same order
* <0: if the first array is lexicographically less than the second array
* \>0: if the first array is lexicographically greater than the second array

Please note:

If a is null and b is not null, then compare method returns -1.
If b is null and a is not null, then compare method returns 1.
If both a and b are null, then compare method returns 0.

#### MISMATCH
find and return the index of the first mismatch between two primitive arrays, otherwise return -1 if no mismatch is found. 
The index will be in the range of 0 (inclusive) up to the length (inclusive) of the smaller array. 
It throws NullPointerException, if either array is null.

#### EQUALS
return true if both of the arrays are null or both contain the same elements in the same order, otherwise return false.


### SQL Procedure
```java
connection.prepareCall("{call <procedure-name>[(<arg1>,<arg2>, ...)]}");
```


### Compare e comparatori


#### Interfaccia Comparable\<T>
Determina un ordine naturale degli oggetti implementando il metodo:

```java
int compareTo(T other)
```
viene usato automaticamente nelle liste automaticamente ordinate come i SortedSet e
la sua implementazione specifica la logica della differenza nell'ordine naturale del this rispetto all'other

* this compareTo other è n < 0 -> il this viene prima dell'other di n posizioni
* this compareTo other è n > 0 -> il this viene dopo dell'other di n posizioni
* this compareTo other è n == 0 -> il this è uguale all'other nell'ordine



#### Interfaccia Comparator\<T>
implementa l'interfaccia che definisce un comparatore da usare eventualmente 
per passarlo ad altri metodi per ordinare l'oggetto secondo una logica diversa dal compareTo (ma anche uguale)
```java
public int compare(T a, T b)
```
restituisce l'ordine naturale di a rispetto a b in questo comparatore


buona pratica: 0 should always be returned for objects when the .equals() comparisons return true

### Other info

attenzione alla conversione da char a int!! dato che sono compatibili

un metodo statico non può fare l'overrido di un metodo di classe anche se ha la stessa firma (errore di compilazione)
*****************************************************
Compile time constant must be:

declared final
primitive or String
initialized within declaration
initialized with constant expression
