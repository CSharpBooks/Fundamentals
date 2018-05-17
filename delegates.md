# Delegates

One of the most powerful characteristics of C# is that it treats methods as first class objects. Much like you can pass `int`s and `string`s around, you can also pass methods themselves! While they see heavy use in implementing callbacks in other methods, delegates are the basis of another powerful feature: events. Here, we will explore how to define delegates and how to use them to perform callbacks.

Consider the following declaration and assignment of an integer:

```cs
int i = 42;
```

The point is that we can assign `42` to a variable of type `int` because we know that `42` is innately an `int`. It is a fact of nature. But if we are expected to believe that we can pass methods in the same way, what type does a method have?

Consider the following method:

```cs
int Add(int x, int y) => x + y;
```

A type of a method is determined by looking at its method signature. That is to say, it's return type and its argument types. Thus, the method signature of our `Add` method is that it returns an `int` and takes two `int` arguments.

## Using the `delegate` Keyword

There are multiple ways to codify these method signatures into something that we can use in our code, but in this section, we will use the `delegate` keyword to define them.

If we want to define a new delegate that defines a method signature, we can write:

```cs
delegate int Calculation(int x, int y);
```

Note that our delegate defines a return type of `int`, and two `int` arguments. Now that we have our method signature defined, let's put it to use.

```cs
delegate int Calculation(int x, int y);

int Add(int x, int y) => x + y;

void Main()
{
    Calculation calc = Add;
}
```

Note that we do not add the parentheses to `Add` when we assign it to `calc` because we do not want to call it, but we instead want refer to the method itself.

Now that we have assigned our `Add` method to `calc`, we can also call `calc` as if it itself were a method:

```cs
Calculation calc = Add;
int result = calc(1, 2); // result is 3
```

## Using `Func<>` and Friends

The `delegate` keyword remains popular when defining events, but delegates have many more uses than just events. As you can imagine, having to explicitly define a delegate every time you want to pass a method around can get a little tiresome.

In C# 3.0, `Func<>` and `Action<>` were introduced.

`Func<>` allows us to define a method signature's argument types and its return types, as expected. Instead of defining a separate `Calculation` delegate, we can simply stick it directly onto `calc`:

```cs
Func<int, int, int> calc;
```

The first two type arguments in `Func<int, int, int>` are the argument types. The very last `int` is the return type. [See the definition of this particular delegate on MSDN.](https://msdn.microsoft.com/en-us/library/bb534647) Also note the *Syntax* block on the link; `Func<>` itself is defined with the `delegate` keyword.

Let's consider a more complete example with `Func<>`:

```cs
int Add(int x, int y) => x + y;

void Main()
{
    Func<int, int, int> calc = Add;
    int result = calc(1, 2); // result is 3
}
```

`Func<>` always has at least one return type, so you can not use it to capture a method definition for something that returns `void`. That is where `Action<>` comes in.

```cs
void ShowNumberOnScreen(int value) => Console.WriteLine(value);

void Main()
{
    Action<int> doSomething = ShowNumberOnScreen;
    doSomething(42); // prints "42" on the console
}
```
