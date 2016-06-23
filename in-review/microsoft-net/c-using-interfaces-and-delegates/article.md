Have you encountered code with a lot of if-else logic? I am sure you have. Most of the time we see that kind of logic, behind the scenes, it's a strategy problem. Someone trying to figure out a strategy to solve the problem using the most primitive language tool - 'if' statement. 

However, when it comes to Object Oriented Programming, it's important to understand that the code needs to be reusable, testable and at the same time scalable. That's where a Strategy Pattern comes in handy. Let's take a look at an example. (Examples are in C# but the concept can be applied to any Object-Oriented Language)

<br>
First, we will see the code for something that is written in the old fashion.


```CSharp
public int Calculate(int x, int y)
{
	if(action == "Add")
    {
    	return x+y;
    }
    else if(action == "Sub")
    {
    	return x-y;
    }
    else if
      ......
      .... and so on.
}
```
<br>

So, problems with this approach: 

1. If you were to add another Strategy (let's say Divide), you will have to change the code in the class. That means you will have to modify the class. However, that is not a good practice. It violates the principle of OCP or Open Closed Principle. OCP says that the classes are Open for Extension but NOT for Modification. 

2. Too many 'if' blocks down the line. 

3. Hard-coded strings. I am sure no one likes those. Counter: We can use Enums, yes but if we can do without using Enums, why add another dependency.

4. Removing a strategy becomes hard, too. 

<br>
So, now let's focus on a good way to do it. Or use the Strategy Pattern - a pattern that let's you choose the strategy on the fly. The strategy depends on the type of the object. The principle behind this pattern is - "Program to an Interface and not to an Implementation". That makes the solution easy to test, extensible and maintainable. 

<br>
We start by defining the Interface. 
```CSharp
public interface ICalculate
{
	public int Calculate(int x, int y);
}
  ```
 <br>

Now we can implement this interface. 

```CSharp
public class Add : ICalculate
{
    #region ICalculate Members

    public int Calculate(int x, int y)
    {
        return x + y;
    }

    #endregion
}

public class Subtract : ICalculate
{
    #region ICalculate Members

    public int Calculate(int x, int y)
    {
        return x * y;
    }

    #endregion
}
```
<br>

Now, when we use this code, this is how we call the code. 
```CSharp
ICalculate calculate = new Add(); int sum = calculate.Calculate(a , b);
```

<br>
Since the object is of type Add, we don't have to check for the type(as we did above) to call the appropriate function. So, at run time, the Calculate method from the class 'Add' gets called. 

Here are some observations: 1. How about adding a new Strategy. It's as simple as adding another class, for example, Multiply. We just need to make sure that it implements the interface ICalculate. 

```CSharp
public class Multiply : ICalculate
{
    #region ICalculate Members

    public int Calculate(int num1, int num2)
    {
        return num1 * num2;
    }

    #endregion
}
```

<br>
1. This does not violate the OCP. In fact, it upholds it.

2. If we need to remove a strategy, we can remove that class from our code and it won't affect any other code (like it would have in the legacy code- we would have to remove that if-else statement). 

3. It has no hard-coded strings. 

<br>
#### Bonus: How about coding Strategy Pattern using delegates?

It's very simple. 
```CSharp
public class Calculator
{
    public static int Add(int x, int y)
    {
        return x + y;
    }

    public static int Subtract(int x, int y)
    {
        return x - y;
    }

    public static int Multiply(int x, int y)
    {
        return x * y;
    }

    public static int Divide(int x, int y)
    {
        return x / y;
    }
}
 ```
 
<br>
Now, we can use Delegates to call this code. 
```CSharp
Func calculate = Calculator.Add;
int sum = calculate(100, 50);
```

or 
```CSharp
calculate = Calculator.Subtract;
int difference = calculate(100, 50);
 ```
<br>

Is this too many lines of code. Well, in that case, we can just code the method inline. Now, we don't' need the class Calculator. 
```CSharp
Func calculate;
calculate = (x, s) => x + s;
```

<br>
Delegates vs Interfaces: Whereas delegates can reduce the code base and increase readability of code, you have to be careful on how you use it otherwise you might end up sacrificing testability. 
						