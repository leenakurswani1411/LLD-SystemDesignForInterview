## Tell me about the Adapter Design Pattern.

**The Adapter Design Pattern, also known as the Wrapper pattern, is a structural design pattern that allows objects with incompatible interfaces to work together. 
It acts as a bridge between two incompatible interfaces. This pattern involves a single class, the Adapter, which joins functionalities of independent or incompatible interfaces.**


### Scenario: 
Consider a scenario where you have a new system that operates with interfaces different from those of existing classes.
An adapter can be used to make these existing classes compatible with the new system without modifying their source code.

### Components:
1. **Target**: The interface that the client expects or works with.
2. **Adaptee**: The class that needs adapting; it has the functionality we need but an incompatible interface.
3. **Adapter**: This class implements the target interface and contains a reference to an Adaptee object. It translates calls to the target interface into calls to the Adaptee's interface.

### Example in Java:
#### Adaptee ClassSuppose you have an `XmlParser` class (Adaptee) that parses XML data, but your application is designed to work with a `JsonParser` interface.
```java
// Adaptee
public class XmlParser {
   public void parseXml(String xmlData) {
      System.out.println("Parsing XML: " + xmlData);
   }
}
```

#### Target InterfaceThe `JsonParser` interface is what your application expects.
```java
// Target Interface
public interface JsonParser {
    void parseJson(String jsonData);
}
```
#### Adapter ClassThe `XmlToJsonAdapter` class adapts the `XmlParser` to the `JsonParser` interface.
```java
// Adapter
public class XmlToJsonAdapter implements JsonParser {
    private XmlParser xmlParser;
    public XmlToJsonAdapter(XmlParser xmlParser) {
      this.xmlParser = xmlParser;
    }

    @Override
    public void parseJson(String jsonData) {
    // Convert JSON to XML (simple simulation)
      String xmlData = "Converted to XML: " + jsonData;
      xmlParser.parseXml(xmlData);
    }
}
```
#### Using the Adapter```javapublic class AdapterDemo {    public static void main(String[] args) {        JsonParser parser = new XmlToJsonAdapter(new XmlParser());        parser.parseJson("{ 'key': 'value' }");    }}```
### Explanation:- **XmlParser (Adaptee)**: This is the existing class with functionality we need but with an incompatible interface.- **JsonParser (Target)**: The interface expected by the client.- **XmlToJsonAdapter (Adapter)**: Implements `JsonParser` and translates the `parseJson` method call into the `XmlParser`'s `parseXml` method. It adapts the `XmlParser` to the `JsonParser` interface.- **AdapterDemo**: Demonstrates using the adapter where the client expects a JSON parser, but the actual work is done by an XML parser through the adapter.
The Adapter pattern is useful for integrating new features or systems with existing codebases, ensuring compatibility without the need to refactor or rewrite existing code.
