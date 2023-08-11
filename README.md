# studio-test-ocp-java11

## Abstract class
Abstract methods in an abstract class cannot be private 

## Annotation
Retention policy: determina quando le annotazioni devono essere scartate

Tipi di retention policy: 

* SOURCE: will be retained only with source code, and discarded during compile time. 
Such annotations are used with build-tools that scan the file.
* CLASS: will be retained till compiling the code (inserted in .class file), and discarded during runtime (DEFAULT).
* RUNTIME: will be available to the JVM through runtime. 
this annotation should be available via reflection at runtime.

### Marker annotation

A marker annotation is one that contains no elements, optional or required. example: @SafeVarargs, @Override, @Documented, and @FunctionalInterface 



## Array methods

### COMPARE
compare methods of Arrays class compare the arrays lexicographically and return below values:

* 0:  if the first and second array are equal and contain the same elements in the same order
* <0: if the first array is lexicographically less than the second array
* \>0: if the first array is lexicographically greater than the second array

Please note:

If a is null and b is not null, then compare method returns -1.
If b is null and a is not null, then compare method returns 1.
If both a and b are null, then compare method returns 0.

### MISMATCH
find and return the index of the first mismatch between two primitive arrays, otherwise return -1 if no mismatch is found. 
The index will be in the range of 0 (inclusive) up to the length (inclusive) of the smaller array. 
It throws NullPointerException, if either array is null.

### EQUALS
return true if both of the arrays are null or both contain the same elements in the same order, otherwise return false.


## SQL Procedure
```java
connection.prepareCall("{call <procedure-name>[(<arg1>,<arg2>, ...)]}");
```


## Compare e comparatori


### Interfaccia Comparable\<T>
Determina un ordine naturale degli oggetti implementando il metodo:

```java
int compareTo(T other)
```
viene usato automaticamente nelle liste automaticamente ordinate come i SortedSet e
la sua implementazione specifica la logica della differenza nell'ordine naturale del this rispetto all'other

* this compareTo other è n < 0 -> il this viene prima dell'other di n posizioni
* this compareTo other è n > 0 -> il this viene dopo dell'other di n posizioni
* this compareTo other è n == 0 -> il this è uguale all'other nell'ordine



### Interfaccia Comparator\<T>
implementa l'interfaccia che definisce un comparatore da usare eventualmente 
per passarlo ad altri metodi per ordinare l'oggetto secondo una logica diversa dal compareTo (ma anche uguale)
```java
public int compare(T a, T b)
```
restituisce l'ordine naturale di a rispetto a b in questo comparatore


buona pratica: 0 should always be returned for objects when the ```.equals()``` comparisons return true

## Console
```java
Console c = System.console();
c.printf(c.readLine("hoh"));
```
readLine stampa "hoh" sulla console e aspetta che venga inserito l'input dall'utente per restituire il risultato
successivamente il printf prende il risultato e lo stampa alla prossima riga

Read and write operations are synchronized to guarantee the atomic completion of critical operations



## Constructor
The constructor will:

* Call ```super()```
* Execute any inline initializations and anonymous initialization blocks.
* Execute the remaining body of the constructor.

A constructor should have ```super(...)``` or ```this(...)``` but not both or else causes compilation failure.


## Enum
It is a compile-time error if a constructor declaration in an enum declaration is public or protected.

It is a compile-time error if a constructor declaration in an enum declaration contains a superclass constructor invocation statement.

It is a compile-time error to refer to a static field of an enum type from a constructor, instance initializer, 
or instance variable initializer of the enum type, unless the field is a constant variable.


## Exceptions

multiple catch statement should have most specialized exception first, example:

```FileNotFoundException extends IOException``` and hence catch block of ```FileNotFoundException``` should appear before the catch block of ```IOException```


In Java exceptions under ```Error``` and ```RuntimeException``` classes are UNCHEKED exceptions they don't need to be catched or declared for the code to compile
all other exceptions are CHECKED

Java doesn't allow to catch specific checked exceptions if these are not thrown by the statements inside 
```java
try {
  System.out.println(1);
}
catch(FileNotFoundException ex) { ... }
```
causes compilation error in this case as ```System.out.println(1)``` will never throw ```FileNotFoundException```
*******************
According to overriding rules, if super class / interface method declares to throw a checked exception, then overriding method of sub class / implementer class has following options:

1. May not declare to throw any checked exception
2. May declare to throw the same checked exception thrown by super class / interface method: SQLException is a valid option.
3. May declare to throw the sub class of the exception thrown by super class / interface method: SQLWarning is a valid option.
4. Cannot declare to throw the super class of the exception thrown by super class / interface method: Exception, Throwable are not valid options.
5. Cannot declare to throw unrelated checked exception: java.io.IOException is not a valid option as it is not related java.sql.SQLException in multi-level inheritance.
6. May declare to throw any RuntimeException or Error: RuntimeException, NullPointerException and Error are valid options.



## ExecutorService
```java
<T> List<Future<T>> invokeAll​(Collection<? extends Callable<T>> tasks, long timeout, TimeUnit unit) throws InterruptedException
```

esegue n thread in tempo t, viene chiamato su un executor service ad esempio
```java
ExecutorService service = Executors.newFixedThreadPool(2);
```

se scade il tempo lancia un eccezione

## Files
```java
Files.writeString(Path p, String s)
```
se il file indicato dal path non è presente, crea un nuovo file e scrive la stringa s nel file
se il file è già presente, sostituisce il suo contenuto con la stringa s

## Functional Interfaces
Functional interface can have 
* constant variables
* static methods
* default methods
* private methods
* can override abstract methods ```equals(Object)``` and ```toString()``` from ```Object``` class

## Interfaces
Variables declared inside interface are implicitly```public```, ```static``` and ```final```
a "variable" can be invoked by using reference variable or ```InterfaceName.variable```

```abstracts``` methods can be ONLY ```public``` and if not, they are implicity public

```default``` modifier is not allowed with ```private``` method of the interface

## Inheritance
le variabili vengono accedute tramite la reference (tipo della classe nel lato sinistro dell'inizializzazione)

There are 2 rules related to return types when overriding a method (for example abstract class/interface):

1. If return type of overridden method is of primitive type, then overriding method should use same primitive type.
2. If return type of overridden method is of reference type, then overriding method can use same reference type or its sub-type (also known as covariant return type).

## JDBC Driver Types

Type 1: Drivers that implement the JDBC API as a mapping to another data access API, 
such as ODBC (Open Database Connectivity). Drivers of this type are generally dependent on a native library, 
which limits their portability. The JDBC-ODBC Bridge is an example of a Type 1 driver.

Type 2: Drivers that are written partly in the Java programming language and partly in native code. 
These drivers use a native client library specific to the data source to which they connect. 
Again, because of the native code, their portability is limited. 
Oracle's OCI (Oracle Call Interface) client-side driver is an example of a Type 2 driver.

Type 3: Drivers that use a pure Java client and communicate with a middleware server using a database-independent protocol. 
The middleware server then communicates the client's requests to the data source.

Type 4: Drivers that are pure Java and implement the network protocol for a specific data source. 
The client connects directly to the data source.


## Label Loop

```java
label1:
    for (; ; ) {
        label2:
        for (; ; ) {
            if (condition1) {
                // break outer loop
                break label1;
            }
            if (condition2) {
                // break inner loop
                break label2;
            }
            if (condition3) {
                // break inner loop
                break;
            }
        }
    }
```
i labelconsentono di fare il break di loop specifici

## Lambda Expressions
When a lambda expression targets a functional interface, all the parameters must be EXACTLY the same. 
The only exception is that an array type is interchangeable with a varargs.

```java
interface Hashable {
  int hash(Object o, Integer i, String... a);
}

Hashable h1 = (Object o, Integer i, String[] a) -> o.hashCode() + i + a.length; // OK
```

## Resource Bundle

quando una classe estende ```ResourceBundle``` allora è possibile cercare con 
```java
ResourceBundle.get("nomeClasse", "parametro");
```

## Map

The ```compute``` method works as following.
1. If old value for the specified key and new value computed by remapping function both are not null, 
in this case old value will be replaced by new value.
2. If old value for the specified key is not null but new value computed by remapping function is null, 
in this case the entry for the specified key will be deleted.
3. If old value for the specified key is null but new value computed by remapping function is not null, 
in this case old value will be replaced by new value.
4. If old value for the specified key is null and new value computed by remapping function is also null, 
in this case the entry for the specified key will be deleted.
```java
Map<String, String> map = new HashMap<>();
map.compute("Jane", (s1, s2) -> "Hi " + s2);
```

sulla chiave "Jane" prova ad associare il valore restituito dalla BiFunction<String>(s1, s2) in cui s1 è la chiave e s2 è il valore,
se restituisce un valore null allora rimuove la chiave jane dalla map


## List methods

```list.remove(0)``` returns the object of list

https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html

```Arrays.asList(...)``` method returns a fixed-size list

```List.of(...)``` method returns unmodifiable list

## Modules
Syntax of qualified exports is:
```java
exports <package_name> to <comma_separated_list_of_modules>;
```
esporta il package solo nel modulo dichiarato (utile ad es. in un modulo aggregatore che ha più requires di moduli con questa sintassi)


```requires <nome_modulo>``` -> rende disponibile i package esportati dal modulo <nome_modulo> al modulo in cui si dichiara
```requires transitive <nome_modulo>``` -> come il requires ma i moduli che richiedono il modulo in cui viene dichiarato automaticamente avranno disponibile anche i package di <nome_modulo>


### Aggregator Modules
An aggregator module collects and re-exports the content of other modules but adds no content of its own
for example ```java.se``` contains all JDK -> contengono tutti ```requires transitive```...




```import``` statements are allowed in module descriptor file -> To have ```import java.sql.*;``` in the module-info.java file, ```requires java.sql;``` directive is needed


In the module descriptor file, if package specified in the exports directive is empty or doesn't exist, then compiler complains about it.

module-info.java cannot be empty, it must at least specify the module name

In the module descriptor file, if package specified in the exports directive is empty or doesn't exist, then compiler complains about it.

unnamed package is not allowed in named modules

### Comandi Compilazione & Moduli
Formato comandi COMPILAZIONE
```javac <options> <source files>```

nel \<options>:
* ```--module-source-path <module-source-path>```, ```-p <module-source-path>``` il path radice su cui trovare i diversi moduli (di solito src)
* ```--module <module-name>```, ```-m <module-name>``` compila tutti i .java  del modulo specificato (Multiple module names are separated by "," )
* ```-d <directory>``` specifica il nome della cartella dove mettere i file .class compilati
* ```--module-path <path>```, ```-p <path>``` specifica i path di eventuali moduli richiesti (requires) al modulo da compilare che sono già stati compilati (A platform dependent path separator (```;``` on Windows and ```:``` on Linux/Mac) is used for multiple entries)

When multiple modules with the same name are in different jar files on the module path, first module is selected and rest of the modules with same name are ignored

Formato comandi ESECUZIONE sui moduli per eseguire una classe con un main in un modulo
```java [options] -m <module>[/<mainclass>] [args...]```
```java [options] --module <module>[/<mainclass>] [args...]```

nelle [options] ```--module-path``` or ```-p``` rappresenta una lista di package contenenti moduli o path a moduli


#### JAR
Options of the ```jar``` command:

* ```-c``` or ```--create```: Create the archive
* ```-f``` or ```--file=FILE```: The archive file name
* ```-e``` or ```--main-class=CLASSNAME```: The application entry point for stand-alone applications bundled into a modular, or executable, jar archive
* ```-C DIR```: Change to the specified directory and include the following file

```java --list-modules```  stampa la lista dei system moduls
```java --list-module-resolution``` stampa l'output del module resolution


## Optionals

ciò che è dentro ```.orElse()``` viene eseguito sempre, sia che l'optional ci sia o meno, a differenza di ```.orElse(Consumer c)```


## Overloading

boxing is preferred over variable arguments
this prints DARK:
```java
class Car {
    void speed(Byte val) { //Line n1
        System.out.println("DARK"); //Line n2
    } //Line n3
 
    void speed(byte... vals) {
        System.out.println("LIGHT");
    }
}
 
public class Test {
    public static void main(String[] args) {
        byte b = 10; //Line n4
        new Car().speed(b); //Line n5
    }
}
```
overloading of methods with multi level inheritanche input: lowest in hierarchy take priority

example:
``` java
Class A{
...
  method1(object)
  method1(charsequence)
  method1(string)

}

method1(null) // applica il method1(string)

```

### Overloading Exceptions
```java
interface Multiplier {
    void multiply(int... x) throws SQLException;
}
 
class Calculator implements Multiplier {
    public void multiply(int... x) throws /*INSERT*/ {
    }
}
```
at ```/*INSERT*/``` According to overriding rules, if super class / interface method declares to throw a checked exception, then overriding method of sub class / implementer class has following options:
1. May not declare to throw any checked exception
2. May declare to throw the same checked exception thrown by super class / interface method: SQLException is a valid option.
3. May declare to throw the sub class of the exception thrown by super class / interface method: SQLWarning is a valid option.
4. Cannot declare to throw the super class of the exception thrown by super class / interface method: Exception, Throwable are not valid options.
5. Cannot declare to throw unrelated checked exception: java.io.IOException is not a valid option as it is not related java.sql.SQLException in multi-level inheritance.
6. May declare to throw any RuntimeException or Error: RuntimeException, NullPointerException and Error are valid options.
  

## Stream
```java
int i = list.parallelStream().reduce(1, Integer::sum, (i1, i2) -> i1 * i2);
```

esegue il ```reduce``` su tutti gli elementi della lista in parallelo con ```Integer::sum``` e poi combina tutti i risultati con ```i1*i2```

## Primitive Casting
```java
int value = (int) 3.14f; // 3 
int score = (int) 3.99f; // 3
```

## Other info

attenzione alla conversione da ```char``` a ```int```!! dato che sono compatibili

un metodo statico non può fare l'override di un metodo di classe anche se ha la stessa firma (errore di compilazione)
*****************************************************
Compile time constant must be:

declared final
primitive or String
initialized within declaration
initialized with constant expression
