## Memento Design Pattern

The Memento design pattern is a behavioral pattern that is used to capture and restore an object’s internal state without violating encapsulation. It allows you to save and restore the state of an object to a previous state, providing the ability to undo or roll back changes made to the object.

#### Components of Memento Design Pattern
1. **Originator** : This component is responsible for creating and managing the state of an object. It has methods to set and get the object’s state, and it can create Memento objects to store its state. The Originator communicates directly with the Memento to create snapshots of its state and to restore its state from a snapshot.

2. **Memento** : The Memento is an object that stores the state of the Originator at a particular point in time. It only provides a way to retrieve the state, without allowing direct modification. This ensures that the state remains

3. **Caretaker** : The Caretaker is responsible for keeping track of Memento objects. It doesn’t know the details of the state stored in the Memento but can request Mementos from the Originator to save or restore the object’s state.

4. **Client** : Typically represented as the part of the application or system that interacts with the Originator and Caretaker to achieve specific functionality. The client initiates requests to save or restore the state of the Originator through the Caretaker.

![alt text](memento-1.png)

![alt text](memento-2.png)

### Example
#### Text Edit
*Imagine you’re building a text editor application, and you want to implement an undo feature that allows users to revert changes made to a document. The challenge is to store the state of the document at various points in time and restore it when needed without exposing the internal implementation of the document.*

Originator (Document)
```java
public class Document {
    private String content;

    public Document(String content) {
        this.content = content;
    }

    public void write(String text) {
        this.content += text;
    }

    public String getContent() {
        return this.content;
    }

    public DocumentMemento createMemento() {
        return new DocumentMemento(this.content);
    }

    public void restoreFromMemento(DocumentMemento memento) {
        this.content = memento.getSavedContent();
    }
}
```

Mementor (DocumentMemento)
```Java
public class DocumentMemento {
    private String content;

    public DocumentMemento(String content) {
        this.content = content;
    }

    public String getSavedContent() {
        return this.content;
    }
}
```

Caretaker (History)
```java
import java.util.ArrayList;
import java.util.List;

public class History {
    private List<DocumentMemento> mementos;

    public History() {
        this.mementos = new ArrayList<>();
    }

    public void addMemento(DocumentMemento memento) {
        this.mementos.add(memento);
    }

    public DocumentMemento getMemento(int index) {
        return this.mementos.get(index);
    }
}

```

Client
```java
public class Main {
    public static void main(String[] args) {
        Document document = new Document("Initial content\n");
        History history = new History();

        // Write some content
        document.write("Additional content\n");
        history.addMemento(document.createMemento());

        // Write more content
        document.write("More content\n");
        history.addMemento(document.createMemento());

        // Restore to previous state
        document.restoreFromMemento(history.getMemento(1));

        // Print document content
        System.out.println(document.getContent());
    }
}
```

Output
```
Initial content
Additional content
More content
```


