# Ex
In the Proxy Design Pattern, a class represents the functionality of another class. This pattern involves a proxy class which controls access to an original object, allowing you to perform something either before or after the request gets through to the original object.

Here's a simple example in Java:

Interface: Image

java
Copy code
public interface Image {
    void display();
}
Real Object: RealImage

java
Copy code
public class RealImage implements Image {
    private String fileName;

    public RealImage(String fileName) {
        this.fileName = fileName;
        loadFromDisk(fileName);
    }

    @Override
    public void display() {
        System.out.println("Displaying " + fileName);
    }

    private void loadFromDisk(String fileName){
        System.out.println("Loading " + fileName);
    }
}
Proxy Object: ProxyImage

java
Copy code
public class ProxyImage implements Image {
    private RealImage realImage;
    private String fileName;

    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(fileName);
        }
        realImage.display();
    }
}
Client Code

java
Copy code
public class ProxyPatternDemo {
    
    public static void main(String[] args) {
        Image image = new ProxyImage("test_image.jpg");

        // Image will be loaded from disk
        image.display(); 
        System.out.println("");

        // Image will not be loaded from disk
        image.display(); 
    }
}
In this example, the ProxyImage class acts as a proxy for RealImage. It controls access to RealImage by first checking if it needs to load the image from disk. This is useful in scenarios where loading the image is a resource-intensive operation, and you want to delay it until it's absolutely necessary (lazy loading). The first call to display will load the image, but subsequent calls will display it without reloading.

