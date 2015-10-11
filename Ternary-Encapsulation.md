When writing ternaries in code, all previous valid input before the ternary is evaluated in the ternary expression. This is especially problematic when you have debug output, or use ternaries flexibly in code.

```java
MyObject something = /* some nullable value with a predetermined getter */;
System.out.println("Returned value: " + something == null ? null : something.getValue());
```

In doing this, you come to evaluate the value of `"Returned value: " + something == null`, which boils down to a string of `"Returned value: true"` or `"Returned value: false"`. Per how ternaries work, this will end up simply evaluating to `false` consistently.

If our value isn't null here, that's not much of a problem, our debug will simply output the return of `something.getValue()` (since the initial debug string is evaluated in the ternary, it's not included). However we face the problem of attempting to dereference a null value when it shouldn't be, since what we expect as a `something == null` check is actually equivalent to a consistent `false`.

To solve this problem, always encapsulate ternary operations when they are combined with other operations:

```java
MyObject something = /* some nullable value with a predetermined getter */;
//note new parenthesis
System.out.println("Returned value: " + (something == null ? null : something.getValue()));
```

```java
//Also appropriate, predetermine the value
MyObject something = /* some nullable value with a predetermined getter */;
String out = String.valueOf(something == null ? null : something.getValue());
System.out.println("Returned value: " + out);
```
