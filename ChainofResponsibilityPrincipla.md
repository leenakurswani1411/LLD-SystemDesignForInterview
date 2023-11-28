# Que. Can you please explain to me how the chain of responsibility works with some code references in Java?

**Ans. The Chain of Responsibility pattern is a behavioral design pattern that allows an object to pass a request along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.**

Concept:
1. Handler Interface: This defines a method for handling requests and optionally includes a link to the next handler in the chain.
2. Concrete Handlers: These are specific handlers that implement the handler interface. Each handler decides whether to process the request and/or pass it to the next handler.
3. Client: The client initializes the chain and sends requests to the first handler.
Advantages:
1. Decouples Request Sender and Receivers: The sender of a request is not aware of which object in the chain will handle the request.
2. Dynamic Chain Configuration: The chain can be re-ordered or changed dynamically at runtime.
3. Promotes Single Responsibility Principle: Each handler in the chain handles only a specific type of command.

**Example in Java**:

---Interface for handlers 

@Handler
interface Handler {
    void setNext(Handler handler);
    void handleRequest(String request);
}

---Concrete Handler A

@A
class ConcreteHandlerA implements Handler {
    private Handler next;

    @Override
    public void setNext(Handler handler) {
        this.next = handler;
    }

    @Override
    public void handleRequest(String request) {
        if ("A".equals(request)) {
            System.out.println("Handler A handled the request.");
        } else if (next != null) {
            next.handleRequest(request);
        }
    }
}

---Concrete Handler B

@B
class ConcreteHandlerB implements Handler {
    private Handler next;

    @Override
    public void setNext(Handler handler) {
        this.next = handler;
    }

    @Override
    public void handleRequest(String request) {
        if ("B".equals(request)) {
            System.out.println("Handler B handled the request.");
        } else if (next != null) {
            next.handleRequest(request);
        }
    }
}

---Client class

@C
public class Client {
    public static void main(String[] args) {
        // Create instances of handlers
        Handler handlerA = new ConcreteHandlerA();
        Handler handlerB = new ConcreteHandlerB();

        // Set the next handler for handlerA
        handlerA.setNext(handlerB);

        // Make requests to handlerA
        handlerA.handleRequest("A"); // Handled by HandlerA
        handlerA.handleRequest("B"); // Passed to HandlerB and handled there
    }
}


