# Definition
Design patterns are reusable solutions to common software design problems.
They're not code to copy, but templates and blueprints that helps to
- Organize code
- Improve maintainability
- Communicate intent clearly with other developers

# Gang of Four Patterns

| Category   | Purpose                             | Examples                      |
| ---------- | ----------------------------------- | ----------------------------- |
| Creational | How objects are created             | Singleton, Factory, Builder   |
| Structural | How objects are composed or related | Adapter, decorator, Composite |
| Behavioral | How objects interact                | Observer, Strategy, State     |

# Singleton
## Goal
Ensure a class has **only one instance** and provide a **global access point** to it

## Usage
- Configuration settings
- Logging systems
- Database connections
- Any service, where **multiple instances would break consistency**

```
public class Logger {
    private static Logger instance;

    private Logger() {}  // private constructor

    public static Logger getInstance() {
        if (instance == null) {
            instance = new Logger();
        }
        return instance;
    }
}
```

```
class Logger:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super(Logger, cls).__new__(cls)
        return cls._instance
```

| Pros                           | Cons                               |
| ------------------------------ | ---------------------------------- |
| Saves memory / central control | Can be hard to test (global state) |
| Easy global access             | Risk of multithreading issues      |
## Singleton - antipattern?
- Global state = hidden coupling -> Singletons act like **global variables**
	- Any class can access and modify them
	- This breaks **encapsulation** and creates **tight coupling**.
	- Makes it hard to track who depends on what.
- Hard to Test / Mock -> > Since singletons are globally accessible, you **can’t easily replace them** with mocks in tests.
	- Makes **unit testing** more difficult.
	- You can't isolate components cleanly.
- Breaks the Single Responsibility Principle -> > Singleton **manages its own lifecycle** _and_ does business logic.
	- It has **two reasons to change**.
	- This violates **SRP** (from SOLID principles).

- Thread safety issues -> > If implemented incorrectly (especially in Java/C++), Singleton can cause **race conditions** in multithreaded environments.
	- You may end up with **two instances** if not synchronized.
- Hidden Dependencies -> > Because it's globally accessible, code that uses a Singleton doesn't **declare its dependency**.
	- - Makes code **harder to understand and maintain**.

This is why we use Dependency injection instead

|Singleton|Dependency Injection|
|---|---|
|Global access to shared instance|Explicit, local injection of dependencies|
|Hidden coupling|Clear dependencies in constructor/params|
|Hard to test|Easy to mock and test|
|Manages its own lifecycle|Framework or caller manages lifecycle|
# Factory Method Pattern

## Goal
Let subclasses or logic decide **which class to instantiate**.

## Usage 
- When object creation is **complex or conditional**
- To avoid `if`/`else` or `switch` scattered across code

```
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() { System.out.println("Drawing Circle"); }
}

class Square implements Shape {
    public void draw() { System.out.println("Drawing Square"); }
}

class ShapeFactory {
    public static Shape getShape(String type) {
        if (type.equals("circle")) return new Circle();
        if (type.equals("square")) return new Square();
        return null;
    }
}
```

```
class Circle:
    def draw(self): print("Drawing Circle")

class Square:
    def draw(self): print("Drawing Square")

def get_shape(shape_type):
    if shape_type == "circle":
        return Circle()
    elif shape_type == "square":
        return Square()
```

|Pros|Cons|
|---|---|
|Centralized object creation logic|Still needs `if/else` or map|
|Adds abstraction and flexibility|More boilerplate|
## Enhancements
### Map based Factory Pattern 
 Store object creators (`Supplier`s or lambdas) in a map, keyed by a string or enum.
```
import java.util.*;
import java.util.function.Supplier;

class ShapeFactory {
    private static final Map<String, Supplier<Shape>> registry = new HashMap<>();

    static {
        registry.put("circle", Circle::new);
        registry.put("square", Square::new);
    }

    public static Shape create(String type) {
        Supplier<Shape> creator = registry.get(type.toLowerCase());
        if (creator == null) throw new IllegalArgumentException("Unknown shape: " + type);
        return creator.get();
    }
}
```

### Registration-Based Factory Pattern
Let classes **self-register** with the factory.  
Useful for plug-in systems, dependency injection, or when you want the **type to “announce itself.”**

Each class registers itself into a central factory map **at load time** (via static block or method).

```
interface Command {
    void execute();
}

class CommandFactory {
    private static final Map<String, Supplier<Command>> registry = new HashMap<>();

    public static void register(String name, Supplier<Command> supplier) {
        registry.put(name, supplier);
    }

    public static Command create(String name) {
        Supplier<Command> supplier = registry.get(name);
        if (supplier == null) throw new IllegalArgumentException("Unknown command");
        return supplier.get();
    }
}
```

```
class PrintCommand implements Command {
    static {
        CommandFactory.register("print", PrintCommand::new);
    }

    public void execute() {
        System.out.println("Executing print");
    }
}
```

When PrintCommand is loaded, it registers itself

|Pattern|Best For|
|---|---|
|**Map-based factory**|Clear mapping from names to creators|
|**Registration-based**|Plugins, auto-registration, dynamic modules|
# Builder

## Goal 
Use when you want to **construct complex objects step-by-step**, especially when there are lots of optional fields or variations.

## Usage
- Too many constructor arguments
- Need for readable, customizable creation without multiple constructors

```
class User {
    private final String name;
    private final String email;
    private final int age;

    private User(Builder builder) {
        this.name = builder.name;
        this.email = builder.email;
        this.age = builder.age;
    }

    public static class Builder {
        private String name;
        private String email;
        private int age;

        public Builder setName(String name) {
            this.name = name;
            return this;
        }

        public Builder setEmail(String email) {
            this.email = email;
            return this;
        }

        public Builder setAge(int age) {
            this.age = age;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}
```

| Use                               | Don't use                       |
| --------------------------------- | ------------------------------- |
| Objects with many optional params | Simple classes with 1-2 fields  |
| Immutability + readability        | When you'd be fine with setters |
| Creating different configurations | Objects built in one line       |
## Possible problems
- Overkill for simple objects
- Too much code
	- Double the number of fields, add cainable methods -> code bloat
	- In java
		- Every field is declared twice
		- Every setter must return this
		- Adding fields means editing both classes
- Forgetting valudation
	- Builders let to skip fields -> mandatory fields need to be validated
	- User u = NEW User.Builder().build() -> Nameless, ageles
-  Hard to extend -> You can't always extend a Builder cleanly if the object is **part of a class hierarchy**.
	- Builder pattern requires knowledge of the object’s structure.
	- Inheritance = more complex builders or repetition

# Abstract Factory pattern

## Goal
Use when you need to **create families of related objects**, without hardcoding their classes.

```
interface Button {
    void paint();
}

class WindowsButton implements Button {
    public void paint() { System.out.println("Windows Button"); }
}

class MacButton implements Button {
    public void paint() { System.out.println("Mac Button"); }
}
```

Abstract Factory:
```
interface GUIFactory {
    Button createButton();
}

class WindowsFactory implements GUIFactory {
    public Button createButton() {
        return new WindowsButton();
    }
}

class MacFactory implements GUIFactory {
    public Button createButton() {
        return new MacButton();
    }
}
```

Application
```
class Application {
    private final Button button;

    public Application(GUIFactory factory) {
        this.button = factory.createButton();
    }

    public void render() {
        button.paint();
    }
}
```

# Structural patterns

Patterns that help you **compose objects and classes**, especially when you want to **adapt, extend, or wrap behavior** without changing existing code.

## Usage
- Integrating with legay code
- Wrapping functionality
- Building flexible, plugin-based systems

## Adapter Pattern

### Goal
Converts one interface into another that the client expects.

### Usage 
-  You want to use an existing class, but its interface doesn’t match what you need.
- You’re integrating with legacy APIs or third-party systems.

```
interface MediaPlayer {
    void play(String fileName);
}

class MP3Player implements MediaPlayer {
    public void play(String fileName) {
        System.out.println("Playing MP3: " + fileName);
    }
}

// Adaptee
class VLCPlayer {
    public void playVLC(String fileName) {
        System.out.println("Playing VLC: " + fileName);
    }
}

// Adapter
class VLCAdapter implements MediaPlayer {
    private VLCPlayer vlc = new VLCPlayer();

    public void play(String fileName) {
        vlc.playVLC(fileName);
    }
}
```

```
MediaPlayer player = new VLCAdapter();
player.play("video.vlc");
```

### Examples

- Adapting old logging systems to new interfaces
- Legacy database connectors
- java.util.Arrays.asList()

### Pros
|Benefit|Why It Matters|
|---|---|
|Integrates **legacy or third-party code**|You can’t always change old APIs|
|Avoids rewriting code|Saves time and reduces risk|
|Preserves open/closed principle|You extend behavior **without modifying** source|
|Promotes **composition over inheritance**|More flexible and decoupled design|
### Problems
- Too Many Adapters = Fragile Architecture -> If you find yourself writing a new adapter for every combination of types, it’s a **code smell**.
	- Trying to **glue incompatible designs** instead of refactoring interfaces properly
	- Introducing **accidental complexity**
- Hides Real Incompatibility -> > Adapters sometimes **force integration** where it shouldn't happen.
	- You may successfully make two classes “talk”, but:
		- The adapted behavior might be **incorrect** or **partial**
		- You could introduce **silent bugs**
		**Example**: Adapting a synchronous service to an async interface — but only faking the delay.
- Leads to Leaky Abstractions -> > Poorly designed adapters can expose too much of the adaptee's API.
	- This breaks encapsulation and **defeats the point** of the adapter.
- Adds Runtime Overhead / Extra Layer -> > Not a huge concern in most apps, but be aware:
	- Extra wrappers can make debugging harder
	- You may lose stack trace clarity or profiling insight

| Good Practice                         | Bad Practice                             |
| ------------------------------------- | ---------------------------------------- |
| Clearly isolate external dependencies | Mixing adapter logic with business logic |
| Adapt only once per integration       | Writing 10+ adapters for variations      |
| Keep adapters thin and readable       | Adapters with too much logic (anti-SRP)  |
| Document what's being adapted and why | Silent, magic compatibility              |
## Decorator pattern

### Goal
Adds behavior to objects **dynamically** without modifying their structure.
	

### Usage 
- You want to add features (e.g. logging, caching, compression) **without subclassing**
- You want to use **composition over inheritance**

```
interface Notifier {
    void send(String message);
}

class BasicNotifier implements Notifier {
    public void send(String message) {
        System.out.println("Sending: " + message);
    }
}

class EmailDecorator implements Notifier {
    private final Notifier wrapped;

    public EmailDecorator(Notifier notifier) {
        this.wrapped = notifier;
    }

    public void send(String message) {
        wrapped.send(message);
        System.out.println("→ Sending email: " + message);
    }
}

class SMSDecorator implements Notifier {
    private final Notifier wrapped;

    public SMSDecorator(Notifier notifier) {
        this.wrapped = notifier;
    }

    public void send(String message) {
        wrapped.send(message);
        System.out.println("→ Sending SMS: " + message);
    }
}
```

```
Notifier notifier = new SMSDecorator(new EmailDecorator(new BasicNotifier()));
notifier.send("You have a message.");
```

```
Sending: You have a message.
→ Sending email: You have a message.
→ Sending SMS: You have a message.
```

### Benefits
- **Flexible** and combinable (wrap as many decorators as needed)
- No need to modify base classes
- Supports **open/closed principle** (open for extension, closed for modification)

|Benefit|Why It Matters|
|---|---|
|Adds behavior **without modifying code**|Follows the **open/closed principle**|
|Supports **dynamic composition**|Add or remove features at runtime|
|Promotes **composition over inheritance**|Cleaner than deep subclass trees|
|Chainable, flexible design|Great for things like logging, filtering, UI widgets|
## Problems
- Too many Layers = hard to Debog -> If you chain 5+ decorators, you end up with a “Russian doll” object.
	- Hard to trace which decorator did what
	- Stack traces can get confusing
	- Breakpoints may not hit where you expect

- Tight Coupling to Interfaces -> > All decorators must **exactly match the interface** of the component they wrap.
	- This means any change in the base interface **breaks all decorators**
	- Can be painful to maintain in fast-moving systems
- Unclear Responsibility -> > When many decorators do similar things (like logging, timing, or caching), it becomes unclear:
	- Who does what?
	- In what order?
	- This leads to **unexpected behavior** and **logic duplication**.
- Violation of Liskov Substitution Principle (LSP) -> > Decorators must act **like** the thing they’re wrapping — otherwise the client gets surprised.
```
class CachingQuery implements Query {
    // might not hit the DB at all — different behavior!
}

```
- If behavior diverges too much from the original, it's no longer a **safe substitute**.

| Good Practice                                     | Bad Practice                                        |
| ------------------------------------------------- | --------------------------------------------------- |
| Keep each decorator **single-responsibility**     | Decorators doing multiple unrelated things          |
| Chain a **small number** (2–3 max)                | 5+ decorators -> problems                           |
| Document the **expected order**                   | Letting order change silently                       |
| Ensure decorators **preserve interface contract** | Violating LSP (e.g., not calling wrapped component) |