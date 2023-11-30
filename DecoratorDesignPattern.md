The Decorator Design Pattern allows for dynamically adding behavior to objects without affecting the behavior of other objects from the same class. It's widely used in Java, particularly for I/O operations. A real-world example is decorating a BufferedReader around a FileReader to add buffering capabilities to file reading operations.

### Real-World Example: Coffee Shop
Imagine a coffee shop where you can order a base coffee and then add various extras (like milk, sugar, whipped cream) to customize it.

### Components:
1. **Component (Coffee)**: An interface defining the behavior (methods) of the coffee.
2. **Concrete Component (SimpleCoffee)**: Basic implementation of the coffee interface.
3. **Decorator (CoffeeDecorator)**: Abstract class implementing the Coffee interface, with a reference to a Coffee object.
4. **Concrete Decorators (Milk, Sugar, etc.)**: These classes extend the Decorator class, adding new behavior (like adding milk or sugar).

### Implementation in Java:

#### Coffee Interface
java
public interface Coffee {
    String getDescription();
    double cost();
}


#### Concrete Coffee
java
public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double cost() {
        return 2.00;
    }
}


#### Coffee Decorator Abstract Class
java
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    public String getDescription() {
        return coffee.getDescription();
    }

    public double cost() {
        return coffee.cost();
    }
}


#### Concrete Decorators
java
public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", with milk";
    }

    @Override
    public double cost() {
        return coffee.cost() + 0.50;
    }
}

public class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", with sugar";
    }

    @Override
    public double cost() {
        return coffee.cost() + 0.25;
    }
}


#### Using the Decorator Pattern
java
public class CoffeeShop {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        coffee = new MilkDecorator(coffee);
        coffee = new SugarDecorator(coffee);

        System.out.println(coffee.getDescription() + " Cost: $" + coffee.cost());
    }
}


### Explanation:
- *SimpleCoffee*: Represents a basic coffee without any extras.
- *MilkDecorator and SugarDecorator*: Add specific behavior (milk and sugar) to the coffee. They modify getDescription and cost methods to reflect their additions.
- *CoffeeShop*: Demonstrates creating a SimpleCoffee and then decorating it with milk and sugar.

This example illustrates the Decorator Pattern's ability to add new functionalities to objects dynamically and flexibly. The pattern is particularly useful when extending functionality through subclassing is impractical or when you want to add or remove responsibilities from an object at runtime.
