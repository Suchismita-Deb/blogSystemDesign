### Decorator Design Pattern.

When you need to add functionality to an object dynamically at run time and subclassing would lead to an explosion of classes then Decorator pattern.

Suppose there is a base `Pizza` class. If a user wants cheese, you may create a `CheesePizza` subclass. If another user wants mushroom, you may create a `MushroomPizza` subclass. Then for cheese + mushroom, cheese + tomato, or mushroom + tomato, you would need many more subclasses. This creates too many class combinations. To avoid that, we use the Decorator Pattern.

**Problem: Without Decorator Pattern (Class Explosion)**

``` java
// ==========================================
// PROBLEM: Subclassing leads to class explosion
// Each combination of toppings needs a new class
// Java doesn't support multiple inheritance!
// ==========================================

// Base interface
interface Pizza {
    String getName();
    double getCost();
}

// Basic pizza with no toppings (Base class)
class BasicPizza implements Pizza {
    @Override
    public String getName() {
        return "Basic Pizza";
    }

    @Override
    public double getCost() {
        return 100.0;
    }
}

// ==========================================
// Single topping pizzas - extend BasicPizza
// ==========================================

class CheesePizza extends BasicPizza {
    @Override
    public String getName() {
        return super.getName() + " + Cheese";
    }

    @Override
    public double getCost() {
        return super.getCost() + 20.0;
    }
}

class MushroomPizza extends BasicPizza {
    @Override
    public String getName() {
        return super.getName() + " + Mushroom";
    }

    @Override
    public double getCost() {
        return super.getCost() + 30.0;
    }
}

class TomatoPizza extends BasicPizza {
    @Override
    public String getName() {
        return super.getName() + " + Tomato";
    }

    @Override
    public double getCost() {
        return super.getCost() + 15.0;
    }
}

// ==========================================
// PROBLEM STARTS HERE: Combinations explode!
// Which class to extend? Arbitrary choice!
// Can't extend both CheesePizza AND MushroomPizza
// ==========================================

// CheeseMushroomPizza extends CheesePizza (arbitrary choice)
// Why not extend MushroomPizza? No good answer!
class CheeseMushroomPizza extends CheesePizza {
    @Override
    public String getName() {
        return super.getName() + " + Mushroom";
    }

    @Override
    public double getCost() {
        return super.getCost() + 30.0; // Duplicate mushroom cost logic
    }
}

class CheeseTomatoPizza extends CheesePizza {
    @Override
    public String getName() {
        return super.getName() + " + Tomato";
    }

    @Override
    public double getCost() {
        return super.getCost() + 15.0; // Duplicate tomato cost logic
    }
}

class MushroomTomatoPizza extends MushroomPizza {
    @Override
    public String getName() {
        return super.getName() + " + Tomato";
    }

    @Override
    public double getCost() {
        return super.getCost() + 15.0; // Duplicate tomato cost logic again!
    }
}

// Triple topping - extends CheeseMushroomPizza (arbitrary again)
class CheeseMushroomTomatoPizza extends CheeseMushroomPizza {
    @Override
    public String getName() {
        return super.getName() + " + Tomato";
    }

    @Override
    public double getCost() {
        return super.getCost() + 15.0; // Duplicate tomato cost logic again!
    }
}

// Main class to test
public class PizzaWithoutDecorator {
    public static void main(String[] args) {
        Pizza basic = new BasicPizza();
        Pizza cheese = new CheesePizza();
        Pizza mushroom = new MushroomPizza();
        Pizza cheeseMushroom = new CheeseMushroomPizza();
        Pizza all = new CheeseMushroomTomatoPizza();

        System.out.println(basic.getName() + " -> ₹" + basic.getCost());
        System.out.println(cheese.getName() + " -> ₹" + cheese.getCost());
        System.out.println(mushroom.getName() + " -> ₹" + mushroom.getCost());
        System.out.println(cheeseMushroom.getName() + " -> ₹" + cheeseMushroom.getCost());
        System.out.println(all.getName() + " -> ₹" + all.getCost());
    }
}
```

**Output:**
```
Basic Pizza -> ₹100.0
Basic Pizza + Cheese -> ₹120.0
Basic Pizza + Mushroom -> ₹130.0
Basic Pizza + Cheese + Mushroom -> ₹150.0
Basic Pizza + Cheese + Mushroom + Tomato -> ₹165.0
```

**Issues with this approach:**

| Problem | Impact |
|---------|--------|
| **Class Explosion** | For 3 toppings we need 7 classes. For 10 toppings we need 1023 classes (2^n - 1). |
| **No Multiple Inheritance** | Java doesn't allow extending both `CheesePizza` and `MushroomPizza`. Forced to make arbitrary choices. |
| **Code Duplication** | Tomato cost logic (`+ 15.0`) is duplicated in `CheeseTomatoPizza`, `MushroomTomatoPizza`, and `CheeseMushroomTomatoPizza`. |
| **Hard to Maintain** | Adding a new topping means creating many new combination classes. |
| **Not Dynamic** | Toppings cannot be added or removed at runtime. |
| **Violates OCP** | Adding new functionality requires modifying existing code structure. |

This is why we need the **Decorator Pattern** — to add behavior dynamically without creating a new class for every combination.
```java
// Base interface
interface Pizza {
    String getName();
    double getCost();
}

// Concrete base pizza
class BasicPizza implements Pizza {
    @Override
    public String getName() {
        return "Basic Pizza";
    }

    @Override
    public double getCost() {
        return 100.0;
    }
}

// ==========================================
// Decorator abstract class
// ==========================================
abstract class PizzaDecorator implements Pizza {
    protected final Pizza decoratedPizza;

    protected PizzaDecorator(Pizza pizza) {
        this.decoratedPizza = pizza;
    }

    @Override
    public String getName() {
        return decoratedPizza.getName();
    }

    @Override
    public double getCost() {
        return decoratedPizza.getCost();
    }
}

// ==========================================
// Concrete decorators (toppings)
// ==========================================
class CheeseDecorator extends PizzaDecorator {
    public CheeseDecorator(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getName() {
        return super.getName() + " + Cheese";
    }

    @Override
    public double getCost() {
        return super.getCost() + 20.0;
    }
}

class MushroomDecorator extends PizzaDecorator {
    public MushroomDecorator(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getName() {
        return super.getName() + " + Mushroom";
    }

    @Override
    public double getCost() {
        return super.getCost() + 30.0;
    }
}

class TomatoDecorator extends PizzaDecorator {
    public TomatoDecorator(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getName() {
        return super.getName() + " + Tomato";
    }

    @Override
    public double getCost() {
        return super.getCost() + 15.0;
    }
}

// ==========================================
// Main class to test
// ==========================================
public class PizzaWithDecorator {
    public static void main(String[] args) {
        Pizza basic = new BasicPizza();
        Pizza cheese = new CheeseDecorator(basic);
        Pizza mushroom = new MushroomDecorator(basic);
        Pizza cheeseMushroom = new MushroomDecorator(new CheeseDecorator(basic));
        // First CheeseDecorator adds Cheese, then MushroomDecorator wraps that result and adds Mushroom. 
        // Each decorator calls super.getName() and super.getCost() from the pizza it wraps.
        Pizza all = new TomatoDecorator(new MushroomDecorator(new CheeseDecorator(basic)));
        // The decorator way with adding all separately. Each decorator wraps the previous one.
        Pizza cheeseTomato = new BasicPizza();
        cheeseTomato = new CheeseDecorator(cheeseTomato);
        cheeseTomato = new TomatoDecorator(cheeseTomato);
        // Pizza cheeseTomato = new CheeseDecorator(new TomatoDecorator(new BasicPizza()));
        
        System.out.println(basic.getName() + " -> ₹" + basic.getCost());
        System.out.println(cheese.getName() + " -> ₹" + cheese.getCost());
        System.out.println(mushroom.getName() + " -> ₹" + mushroom.getCost());
        System.out.println(cheeseMushroom.getName() + " -> ₹" + cheeseMushroom.getCost());
        System.out.println(all.getName() + " -> ₹" + all.getCost());
        System.out.println(cheeseTomato.getName() + " -> ₹" + cheeseTomato.getCost());
        // The order of wrapping determines the string order of toppings.
        // The wrapped Cheese first, then Tomato, the name is "Basic Pizza + Cheese + Tomato"
    }
}


// Output.
Basic Pizza -> ₹100.0
Basic Pizza + Cheese -> ₹120.0
Basic Pizza + Mushroom -> ₹130.0
Basic Pizza + Cheese + Mushroom -> ₹150.0
Basic Pizza + Cheese + Mushroom + Tomato -> ₹165.0
Basic Pizza + Cheese + Tomato -> ₹135.0

```

Similar example.

Create a base coffee with a fixed cost.  
Dynamically add various ingredients (e.g., Milk, Sugar, Whipped Cream) as decorators that adjust the coffee's description and price.  
Display the final description and total price of the coffee with all added ingredients.

```java

public interface Coffee {
    String getDescription();
    double getCost();
}

public class BasicCoffee implements Coffee {

    @Override
    public String getDescription() {
        return "Basic Coffee";
    }

    @Override
    public double getCost() {
        return 3.00;
    }
}

public abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        // TODO: Complete this method to return the description of the decorated coffee.
        return coffee.getDescription();

    }

    @Override
    public double getCost() {
        // TODO: Complete this method to return the cost of the decorated coffee.
        return coffee.getCost();

    }
}
public class Milk extends CoffeeDecorator {

    public Milk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.50;
    }
}
public class Sugar extends CoffeeDecorator {

    public Sugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.30;
    }
}

public class WhippedCream extends CoffeeDecorator {

    public WhippedCream(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", Whipped Cream";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 0.70;
    }
}

public class Exercise {

    // Do not modify the run method. It is designed to demonstrate the usage of the Decorator pattern for customizing coffee with various ingredients.
    public void run(){

        Scanner sc = new Scanner(System.in);

        Coffee coffee = new BasicCoffee();

        boolean addMoreIngredients = true;

        while (addMoreIngredients) {

            String choices = sc.nextLine();
            String[] ingredients = choices.split(" ");

            for (String choice : ingredients) {

                switch (choice) {
                    case "1":
                        // TODO: Complete the implementation for adding Milk to the coffee.
                        coffee = new Milk(coffee);


                        break;
                    case "2":
                        // TODO: Complete the implementation for adding Sugar to the coffee.
                        coffee = new Sugar(coffee);


                        break;
                    case "3":
                        // TODO: Complete the implementation for adding Whipped Cream to the coffee.
                        coffee = new WhippedCream(coffee);


                        break;
                    case "4":
                        addMoreIngredients = false;
                        break;
                    default:
                        System.out.println("Invalid choice: " + choice);
                        break;
                }
            }

            if (!addMoreIngredients) {
                break;
            }
        }

        System.out.println("Final Coffee Description: " + coffee.getDescription());
        System.out.println("Total Cost: $" + coffee.getCost());

        sc.close();
    }
}

```