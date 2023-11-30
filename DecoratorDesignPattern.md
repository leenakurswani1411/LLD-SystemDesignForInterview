## Tell me how decorator design pattern works?

**The Decorator Design Pattern allows for dynamically adding behavior to objects without affecting the behavior of other objects from the same class. It's widely used in Java, particularly for I/O operations. A real-world example is decorating a BufferedReader around a FileReader to add buffering capabilities to file reading operations.**

### Real-World Example: Coffee Shop
Let's create an example where we have a LargeFileReader interface representing the action of reading a large file. The ProxyFileReader will act as a proxy to this file reader, initializing the actual file reading operation only when a method requiring file contents is called.

### Components:
1. **Subject Interface (LargeFileReader)**: An interface defining the read operation.
2. **Real Subject (RealFileReader)**: The actual implementation of the file reader.
3. **Proxy (ProxyFileReader)**: A proxy to the real file reader, which initializes the real reader only when needed.

### Java Implementation:

#### LargeFileReader Interface
```java
public interface LargeFileReader {
    String readContents();
}
```

#### Real FileReader
```java
public class RealFileReader implements LargeFileReader {
    private String filePath;
    public RealFileReader(String filePath) {
        this.filePath = filePath;
        loadFileFromDisk();
    }
    private void loadFileFromDisk() {
        System.out.println("Loading file from disk: " + filePath);
        // Simulate time-consuming operation
    }
    @Override
    public String readContents() {
        return "File contents of " + filePath;
    }
}
```

#### Proxy FileReader
```java
public class ProxyFileReader implements LargeFileReader {
    private String filePath;
    private RealFileReader realFileReader;
    public ProxyFileReader(String filePath) {
        this.filePath = filePath;
    }

    @Override
    public String readContents() {
        if (realFileReader == null) {
            realFileReader = new RealFileReader(filePath);
        }
        return realFileReader.readContents();
    }
}
```

#### Using the Proxy
```java
public class ProxyPatternDemo {
    public static void main(String[] args) {
        LargeFileReader fileReader = new ProxyFileReader("largefile.txt");     
        // File is not loaded at this point
        System.out.println("Proxy initialized, file not loaded yet.");
        // File gets loaded here
        System.out.println(fileReader.readContents());
    }
}
```

### Explanation:
- *RealFileReader*: This class simulates reading a large file. The loadFileFromDisk method represents the loading process.
- *ProxyFileReader*: Acts as a proxy to RealFileReader. It delays the file loading process until the readContents method is called.
- *ProxyPatternDemo*: Demonstrates the use of ProxyFileReader. The file loading operation is deferred until actual data is requested.

This implementation showcases how the Proxy pattern can be used to defer expensive operations, thus improving performance in scenarios where the costly operation might not be needed immediately or at all.
