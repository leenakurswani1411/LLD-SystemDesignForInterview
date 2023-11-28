# Que: Can you explain central logging to me using the Microservices-based example? Or How do we track down a transaction if it is passing through multiple microservices?

**Ans. Absolutely! Let's use the concept of log levels (INFO, WARN, ERROR) in a microservices architecture to illustrate the Chain of Responsibility pattern. In this context, each log level acts as a handler in the chain, processing specific types of log messages.**

### Concept in Context:
- *Handler Interface*: A logger Interface that can log messages and set the next logger in the chain.
- *Concrete Handlers*: Different loggers for INFO( Information), WARN( Warning), and ERROR( Error) levels.
- *Microservice*: Represents a client that sends log messages.

### Advantages in Microservices:
- *Flexibility in Log Processing*: Different loggers can handle different types of messages to log processing.
- *Ease of Maintenance*: Adding or modifying log levels affects only specific parts of the chain, not the entire logging mechanism.
- *Decoupling of Loggers*: Loggers are independent of each other, adhering to microservice principles.

### Java Example:

Handler Interface
```java
interface Logger {
    void setNext(Logger nextLogger);
    void logMessage(String message, LogLevel level);
}
```
```java
enum LogLevel {
    INFO, WARN, ERROR
}
```

Concrete Handlers
```java
class InfoLogger implements Logger {
    private Logger nextLogger;
    @Override
    public void setNext(Logger nextLogger) {
        this.nextLogger = nextLogger;
    }
    @Override
    public void logMessage(String message, LogLevel level) {
        if (level == LogLevel.INFO) {
            System.out.println("INFO: " + message);
        } else if (nextLogger != null) {
            nextLogger.logMessage(message, level);
        }
    }
}
```
```java
class WarnLogger implements Logger {
    private Logger nextLogger;
    @Override
    public void setNext(Logger nextLogger) {
        this.nextLogger = nextLogger;
    }
    @Override
    public void logMessage(String message, LogLevel level) {
        if (level == LogLevel.WARN) {
            System.out.println("WARN: " + message);
        } else if (nextLogger != null) {
            nextLogger.logMessage(message, level);
        }
    }
}
```
```java
class ErrorLogger implements Logger {
    private Logger nextLogger;
    @Override
    public void setNext(Logger nextLogger) {
        this.nextLogger = nextLogger;
    }
    @Override
    public void logMessage(String message, LogLevel level) {
        if (level == LogLevel.ERROR) {
            System.out.println("ERROR: " + message);
        } else if (nextLogger != null) {
            nextLogger.logMessage(message, level);
        }
    }
}
```

Microservice (Client)
```java
public class Microservice {
    public static void main(String[] args) {
        Logger infoLogger = new InfoLogger();
        Logger warnLogger = new WarnLogger();
        Logger errorLogger = new ErrorLogger();
        infoLogger.setNext(warnLogger);
        warnLogger.setNext(errorLogger);
        infoLogger.logMessage("This is an information.", LogLevel.INFO);
        infoLogger.logMessage("This is a warning.", LogLevel.WARN);
        infoLogger.logMessage("This is an error!", LogLevel.ERROR);
    }
}
```

In this example, each logger handles a specific type of log message. If a logger cannot handle a message (e.g., InfoLogger receives a WARN message), it passes the message to the next logger in the chain. This approach aligns well with microservices' principles of modularity and decoupling, allowing for flexible and maintainable logging across different services.
