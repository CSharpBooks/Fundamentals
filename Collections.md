# Collections

Collections allow you to store multiple objects or values in a unified manner. There are multiple types of collections and each one has its strengths and weaknesses.

## Enumerables

All types that implement `IEnumerable<T>` allow us to iterate over them using a `foreach`, allowing us to access every element, one at a time:

```
foreach(var number in numbers)
{
	Console.WriteLine(number);
}
```

## Lists

One of the most basic collection types is the `List<T>`. Let us create a new list of `Person`s:

```
var people = new List<Person>();
```

Now we can add people to the list

```
people.Add(new Person("John Rambo")); // add a single person
people.Add(new Person("Sarah Connor"));
```

We added two people to the list. `people.Count` is equal to 2.
John Rambo got assigned the number (index) 0, and Sarah Connor has an index 1. Note that we start counting from 0.

We can access individual elements of the list through the number using the syntax `list[number]`:

```
Console.WriteLine(people[1].Name); // prints Sarah Connor
```

We can also remove the individual elements from the list by using `RemoveAt`:

```
people.RemoveAt(0);
```

If we run this code, we'll see that Sarah Connor has an index 0, because all the elements after the removed element had to be moved one position before. For very large lists, this will take a *long* time. If we have a lot of elements to remove from a list, we can use the `RemoveAll` function:

```
people.RemoveAll(person => person.DateOfBirth.Year == 1990); // remove all people who have been born in 1990 from the list
```

## Dictionaries

???

## HashSet

???

## SortedSet

???


## Arrays

An array `T[]` is a limited version of `List<T>`. Its size must be known at creation time, and can't be changed later on. Most code doesn't use them directly, but when interacting with code that requires usage of arrays, you can convert any collection to an array by using LINQ's [`Enumerable.ToArray()`](https://docs.microsoft.com/en-gb/dotnet/api/system.linq.enumerable.toarray?view=netframework-4.7.1#System_Linq_Enumerable_ToArray__1_System_Collections_Generic_IEnumerable___0__):

```
var list = new List<string>{};
OverlyRestrictiveFunction(list.ToArray());

```

## Non-generic collections

They are located in `System.Collections`. Since generic collections are infinitely more convenient to use, don't use the non-generic collections. However, some libraries may return instances of non-generic collections, and you'll need to deal with them.

You can create a generic enumerable from a non-generic one by using `.Cast<T>()` and `.OfType<T>()` extension methods.