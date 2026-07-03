### Builder Design Pattern.

When there is an object with many parameters (optional) then the constructor becomes difficult to maintain. 

Long constructor parameter list.  
Not able to understand which parameter is optional and which is mandatory.  
No flexibility when it comes to set limited values.
```java
public class House {
    private String structure;
    private String roof;
    private int rooms;
    private boolean hasSwimmingPool; // optional
    private boolean hasParkingLot; // optional
    
    // Constructor overloading is not possible.
    public House(String roof);
    public House(String structure);
    // The constructor with the same parameter type House(String roof) and House(String structure) not possible they should have different parameter type.
    public House(String structure, String roof, int rooms, boolean hasSwimmingPool, boolean hasParkingLot) {
        this.structure = structure;
        this.roof = roof;
        this.rooms = rooms;
        this.hasSwimmingPool = false;
        this.hasParkingLot = hasParkingLot;
    }
    // Setting the optional parameter with the constructor is not convenient.
}
```

Solution - It separates the construction of an object from its representation - an interface for creating an object.
In order to construct and object we will use a separate class - nested class.

```java
House house = new House.HouseBuilder()
                .setStructure("Concrete")
                .setRoof("Tile")
                .setRooms(3)
                .setHasSwimmingPool(true)
                .setHasParkingLot(false)
                .build();

// It is the set method. Optional parameter can be set and it can be chain in any order. Put the value needed and make the other values default. The build method will return the object.

// The HouseBuilder class will live inside the House class.

// The setStructure and the set methods return a HouseBuilder object. So we can chain the methods. The build method will return the House object.

// Build method will return House.

// Constructor for House - No call to the constructor we will create a private constructor which the builder will use to return.
```