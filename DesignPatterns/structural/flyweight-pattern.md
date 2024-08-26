## Flyweight Design Pattern
The Flyweight design pattern is a structural pattern that aims to minimize memory usage and improve performance by sharing objects that have similar or identical states. It is particularly useful when dealing with many objects that can be grouped into shared flyweight objects, reducing the overall memory footprint.

**Some key concepts of flyweight design pattern**:

- **Shared State**: Objects are divided into intrinsic (shared) state and extrinsic (context-dependent) state. The intrinsic state is shared among multiple objects, while the extrinsic state can vary and is passed to the flyweight objects as needed.
- **Object Reuse**: Instead of creating new objects each time they are needed, Flyweight objects are reused from a pool of existing objects. This reduces the amount of memory used and improves performance.
- **Factory for Flyweights**: A factory class manages the creation and reuse of flyweight objects. It ensures that clients get a shared flyweight object when they request one, or it creates a new one only if necessary.

**Real-World Example of Flyweight Design Pattern**
*Imagine a text editor where each character on the screen needs to be displayed with a specific font, size, and style. Instead of creating a new object for every character with the same formatting, the text editor uses a shared pool of character objects (flyweights) that store common formatting details. This approach saves memory and improves performance by reusing objects, ensuring efficient handling of large amounts of text with varying formatting needs.*

### Components of Flyweight Design Pattern
The Flyweight design pattern typically consists of the following components:

- **Flyweight Interface/Class**:
    - Defines the interface through which flyweight objects can receive and act on extrinsic state.
- **Concrete Flyweight Classes**:
    - Implements the Flyweight interface and represents objects that can be shared.
    - Stores intrinsic state (state that can be shared) and provides methods to manipulate intrinsic state if needed.
- **Flyweight Factory**:
    - Manages a pool of flyweight objects.
    - Provides methods for clients to retrieve or create flyweight objects.
    - Ensures flyweight objects are shared appropriately to maximize reusability.
- **Client**:
    - Uses flyweight objects to perform operations.
    - Maintains or passes extrinsic state to flyweight objects when needed.
    - Does not manage the lifecycle of flyweight objects directly but interacts with them via the factory.

### Example
#### WordProcessor
**Without Flyweight Pattern**
```java
public class Character {
	char character;    // intrinsic
	String fontType;   // intrinsic
	int size;          // intrinsic
	int row;           // extrinsic
	int column;        // extrinsic
	
	Character(char character, String fontType, int size, int row, int column) {
		this.character = character;
		this.fontType = fontType;
		this.size = size;
		this.row = row;
		this.column = column;
	}
	
}

// type "this is test"
public class WordDocument {

	public static void main(String args[]) {
		Character obj1 = new Character('t', "Arial", 12, 0, 1);
		Character obj1 = new Character('h', "Arial", 12, 0, 2);
		Character obj1 = new Character('i', "Arial", 12, 0, 3);
		Character obj1 = new Character('s', "Arial", 12, 0, 4);
		Character obj1 = new Character(' ', "Arial", 12, 0, 5);
		Character obj1 = new Character('i', "Arial", 12, 0, 6);
		Character obj1 = new Character('s', "Arial", 12, 0, 7);
		Character obj1 = new Character(' ', "Arial", 12, 0, 8);
		Character obj1 = new Character('t', "Arial", 12, 0, 9);
		Character obj1 = new Character('e', "Arial", 12, 0, 10);
		Character obj1 = new Character('s', "Arial", 12, 0, 11);
		Character obj1 = new Character('t', "Arial", 12, 0, 12);
		
	}
}
```

In above approach we can see the number of objects are too many

**With Flyweight**

Flyweight Interface
```java
public interface ILetter {
	public void display(int row, int column);
}
```

Concrete Flyweight Class
```java
public class DocumentCharacter implements ILetter {
	private char character;
	private String fontType;
	private int size;
	
	public DocumentCharacter(char character, String fontType, int size) {
		this.character = character;
		this.fontType = fontType;
		this.size = size;
	}
	
	// getters for Intrinsic member variables
	
	@Override
	public void display(int row, int column) {
		// display the character of particular fontType & size at given location
	}
}
```
Flyweight Factory
```java
public class LetterFactory {
	private static Map<Character, ILetter> characterCache = new HashMap<>();
	
	public static ILetter createLetter(Character characterValue) {
		if(characterCache.containsKey(characterValue) {
			return characterCache.get(characterValue);
		}
		else {
			DocumentCharacter charObj = new DocumentCharacter(characterValue, "Arial", 10);
			characterCache.put(characterValue, charObj);
			
			return charObj;
		}
		
	}
}
```
Client
```java
public class Main {
	
	public static void main(String args[]) {
		ILetter obj1 = LetterFactory.createLetter('t');   // new object
		ILetter obj2 = LetterFactory.createLetter('h');   // new object
		ILetter obj3 = LetterFactory.createLetter('i');   // new object
		ILetter obj4 = LetterFactory.createLetter('s');   // new object
		ILetter obj5 = LetterFactory.createLetter(' ');   // new object
		ILetter obj6 = LetterFactory.createLetter('i');   // reference of obj3
		ILetter obj7 = LetterFactory.createLetter('s');   // reference of obj4
		ILetter obj8 = LetterFactory.createLetter(' ');   // reference 0f obj5
		ILetter obj9 = LetterFactory.createLetter('t');   // reference of obj1
		ILetter obj10 = LetterFactory.createLetter('e');  // new object 
		ILetter obj11 = LetterFactory.createLetter('s');  // reference of obj4
		ILetter obj12 = LetterFactory.createLetter('t');  // reference of obj1

		
	}

}
```

![alt text](<flyweight-1.png>)

![alt text](<flyweight-2.png>)

![alt text](<flyweight-3.png>)
