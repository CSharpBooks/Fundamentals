# Understanding the difference between value and reference types



To prevent any possible problems in a code it is important to know how value and reference types work.

In C# there are 15 value types  —

* bool
* byte / sbyte
* char
* decimal
* double
* float
* int / unit
* long / ulong
* short / ushort 
* enum

and any struct. As for the reference types, C# has 3 built-in types — *dynamic*, *object* and *string*. And anything else that is a *class*, *interface*, *array* or *delegate* is also considered a reference type.

### So, what ***is*** the difference between them?

Let's start with value types. The way they work is when you pass such type as an argument the ***value*** is copied. Take a look at the following example.

```csharp
public static void Main(string[] args)
{
    int x = 5;
    int y = x;
    y++;
    Console.WriteLine(x);
}
```

What will the console print? It will print 5. Why? Because method  incremented the field that had the *copy* of a value stored in it. So how does reference work then? Let's use an example again.

```csharp
public class TestClass
{
    public int X { get; set; }
}

public static void Main(string[] args)
{
    var instance1 = new TestClass();
    instance1.X = 5;
    var instance2 = instance1;
    instance2.X = 10;
    Console.WriteLine(instance1.X); //Prints 10
}
```

You may be wondering

> But why did it output 10? Didn't we set it to 5?

Yes, we did. But because class is a reference type, what we actually did by doing

```csharp
var instance2 = instance1;
```

is we assigned the reference to instance2 and now both instance1 and instance2 point to the exact same objects on the heap.

The gif below might help you to clearly understand the difference between those two data types.

![Value vs Reference](https://media.giphy.com/media/xUPGcLrX5NQgooYcG4/giphy.gif)

# ref, in and out Keywords

What if you want to pass a value type by reference? No worries, C# has a special keyword that does just that. It's pretty straightforward in nature. Here's an example.

```csharp
public static void Main(string[] args)
{
    int number = 1;
    ChangeValueByRef(ref number);
    Console.WriteLine(number); // prints 1001
}

private static void ChangeValueByRef(ref int valueType)
{
    valueType = valueType + 1000;
}
```

Seems easy enough, right? Let's move on.



Next on the list is a new ***in*** keyword added in C# 7.2

 ***in*** means that you pass the type by reference, but also doesn't let you modify that reference. This is especially useful for reading data from big structs that will use a lot of memory if copied directly.

```csharp
public static int Add(in int number1, in int number2)
{
	number1 = 5;
	return number1 + number2; //Compile error
    //Cannot assign to variable 'in int' because it is a readonly variable
}
```

***out*** is the exact same thing as ***ref*** except ***ref*** requires the variable to be initialized before it is passed. 

```csharp
public static void Main(string[] args)
{
    int numberToAssignTo;
    ChangeValueByOut(out numberToAssignTo);
    Console.WriteLine(numberToAssignTo); //Prints 123
}

private static void ChangeValueByOut(out int outValue)
{
    outValue = 123;
}
```



