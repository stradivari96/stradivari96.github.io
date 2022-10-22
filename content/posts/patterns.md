---
title: "📝 Design Patterns notes"
date: 2021-08-28
draft: false

aliases:
  - /ddd

tags: ["Software Engineering", "Programming", "Python"]
---

Some notes about design patterns

<!--more-->

![img](https://images.pexels.com/photos/196644/pexels-photo-196644.jpeg?cs=srgb&dl=pexels-picjumbocom-196644.jpg&fm=jpg)

## Behavioral

### [Strategy Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/strategy.py)

> Define a family of algorithms, encapsulate each one, and make them interchangeable.
> Strategy lets the algorithm vary independently from clients that use it.

- HAS-A can be better than IS-A

```
Duck
    - flyBehavior  # classes or functions
    - quackBehavior  # classes or functions
    - swim()
    - display()
    - performFly()
    - performQuack()
```

### [Observer Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/observer.py)

> Maintains a list of dependents and notifies them of any state changes.

```python
# define a generic observer type
class Observer(Protocol):
    def update(self, subject: Subject) -> None:
        ...


class Subject:
    def __init__(self) -> None:
        self._observers: list[Observer] = []

    def attach(self, observer: Observer) -> None:
        if observer not in self._observers:
            self._observers.append(observer)

    def detach(self, observer: Observer) -> None:
        with suppress(ValueError):
            self._observers.remove(observer)

    def notify(self, modifier: Observer | None = None) -> None:
        for observer in self._observers:
            if modifier != observer:
                observer.update(self)
```

### [Command Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/command.py)

> Command pattern decouples the object invoking a job from the one who knows
> how to do it. As mentioned in the GoF book, a good example is in menu items.
> You have a menu that has lots of items. Each item is responsible for doing a
> special thing and you want your menu item just call the execute method when
> it is pressed. To achieve this you implement a command object with the execute
> method for each menu item and pass to it.

```python
class HideFileCommand:
    def __init__(self) -> None:
        self._hidden_files: List[str] = []
    def execute(self, filename: str) -> None:
        self._hidden_files.append(filename)
    def undo(self) -> None:
        filename = self._hidden_files.pop()


class DeleteFileCommand:
    def __init__(self) -> None:
        self._deleted_files: List[str] = []
    def execute(self, filename: str) -> None:
        self._deleted_files.append(filename)
    def undo(self) -> None:
        filename = self._deleted_files.pop()


class MenuItem:
    def __init__(self, command: Union[HideFileCommand, DeleteFileCommand]) -> None:
        self._command = command
    def on_do_press(self, filename: str) -> None:
        self._command.execute(filename)
    def on_undo_press(self) -> None:
        self._command.undo()

item1 = MenuItem(DeleteFileCommand())
item2 = MenuItem(HideFileCommand())

test_file_name = 'test-file'
item1.on_do_press(test_file_name)
item1.on_undo_press()
item2.on_do_press(test_file_name)
item2.on_undo_press()
```

### [Template Method Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/template.py)

> Defines the skeleton of a base algorithm, deferring definition of exact
> steps to subclasses.

```python
def template_function(getter, converter=False, to_save=False) -> None:
    data = getter()
    print(f"Got `{data}`")

    if len(data) <= 3 and converter:
        data = converter(data)
    else:
        print("Skip conversion")

    if to_save:
        saver()

    print(f"`{data}` was processed")
```

### [Iterator Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/iterator.py)

> Provides a way to access the elements of an aggregate object
> sequentially without exposing its underlying representation.

```python
def count_to(count: int):
    """Counts by word numbers, up to a maximum of five"""
    numbers = ["one", "two", "three", "four", "five"]
    yield from numbers[:count]


# Test the generator
def count_to_five() -> None:
    return count_to(5)
```

### [State Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/state.py)

> Allows an object to alter its behavior
> when its internal state changes. The object will appear to
> change its class.

```python
class State:
    """Base state. This is to share functionality"""

    def scan(self) -> None:
        """Scan the dial to the next station"""
        self.pos += 1
        if self.pos == len(self.stations):
            self.pos = 0
        print(f"Scanning... Station is {self.stations[self.pos]} {self.name}")


class AmState(State):
    def __init__(self, radio: Radio) -> None:
        self.radio = radio
        self.stations = ["1250", "1380", "1510"]
        self.pos = 0
        self.name = "AM"

    def toggle_amfm(self) -> None:
        print("Switching to FM")
        self.radio.state = self.radio.fmstate


class FmState(State):
    def __init__(self, radio: Radio) -> None:
        self.radio = radio
        self.stations = ["81.3", "89.1", "103.9"]
        self.pos = 0
        self.name = "FM"

    def toggle_amfm(self) -> None:
        print("Switching to AM")
        self.radio.state = self.radio.amstate


class Radio:
    """A radio. It has a scan button, and an AM/FM toggle switch."""

    def __init__(self) -> None:
        """We have an AM state and an FM state"""
        self.amstate = AmState(self)
        self.fmstate = FmState(self)
        self.state = self.amstate

    def toggle_amfm(self) -> None:
        self.state.toggle_amfm()

    def scan(self) -> None:
        self.state.scan()
```

### [Chain of Responsibility Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/chain_of_responsibility.py)

> The Chain of responsibility is an object oriented version of the
> `if ... elif ... elif ... else ...` idiom, with the
> benefit that the condition–action blocks can be dynamically rearranged
> and reconfigured at runtime.

### [Mediator Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/mediator.py)

> Objects in a system communicate through a Mediator instead of directly with each other.
> This reduces the dependencies between communicating objects, thereby reducing coupling.

### [Memento Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/memento.py)

> Provides the ability to restore an object to its previous state.

### [Visitor Pattern](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/visitor.py)

> Separates an algorithm from an object structure on which it operates.

## Structural

### [Decorator Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/decorator.py)

> The Decorator pattern is used to dynamically add a new feature to an
> object without changing its implementation. It differs from
> inheritance because the new feature is added only to that particular
> object, not to the entire subclass.

- Classes should be open for extension, but closed for modification.

```python
class TextTag:
    def __init__(self, text: str) -> None:
        self._text = text
    def render(self) -> str:
        return self._text


class BoldWrapper(TextTag):
    def __init__(self, wrapped: TextTag) -> None:
        self._wrapped = wrapped
    def render(self) -> str:
        return f"<b>{self._wrapped.render()}</b>"


class ItalicWrapper(TextTag):
    def __init__(self, wrapped: TextTag) -> None:
        self._wrapped = wrapped
    def render(self) -> str:
        return f"<i>{self._wrapped.render()}</i>"
```

### [Adapter Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/adapter.py)

> The Adapter pattern provides a different interface for a class. We can
> think about it as a cable adapter that allows you to charge a phone
> somewhere that has outlets in a different shape. Following this idea,
> the Adapter pattern is useful to integrate classes that couldn't be
> integrated due to their incompatible interfaces.

```python
class Adapter:
    """Adapts an object by replacing methods.
    Usage
    ------
    dog = Dog()
    dog = Adapter(dog, make_noise=dog.bark)
    """

    def __init__(self, obj: T, **adapted_methods: Callable):
        """We set the adapted methods in the object's dict."""
        self.obj = obj
        self.__dict__.update(adapted_methods)

    def __getattr__(self, attr):
        """All non-adapted calls are passed to the object."""
        return getattr(self.obj, attr)

    def original_dict(self):
        """Print original object dict."""
        return self.obj.__dict__
```

### [Facade Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/facade.py)

> The Facade pattern is a way to provide a simpler unified interface to
> a more complex system. It provides an easier way to access functions
> of the underlying system by providing a single entry point.

```python
class ComputerFacade:

    def __init__(self):
        self.cpu = CPU()
        self.memory = Memory()
        self.ssd = SolidStateDrive()

    def start(self):
        self.cpu.freeze()
        self.memory.load("0x00", self.ssd.read("100", "1024"))
        self.cpu.jump("0x00")
        self.cpu.execute()
```

### [Composite Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/composite.py)

> The composite pattern describes a group of objects that is treated the
> same way as a single instance of the same type of object. The intent of
> a composite is to "compose" objects into tree structures to represent
> part-whole hierarchies. Implementing the composite pattern lets clients
> treat individual objects and compositions uniformly.

```python
class Graphic(ABC):
    @abstractmethod
    def render(self) -> None:
        raise NotImplementedError("You should implement this!")


class CompositeGraphic(Graphic):
    def __init__(self) -> None:
        self.graphics: List[Graphic] = []

    def render(self) -> None:
        for graphic in self.graphics:
            graphic.render()

    def add(self, graphic: Graphic) -> None:
        self.graphics.append(graphic)

    def remove(self, graphic: Graphic) -> None:
        self.graphics.remove(graphic)


class Ellipse(Graphic):
    def __init__(self, name: str) -> None:
        self.name = name

    def render(self) -> None:
        print(f"Ellipse: {self.name}")

ellipse1 = Ellipse("1")
ellipse2 = Ellipse("2")
graphic1 = CompositeGraphic()
graphic2 = CompositeGraphic()
graphic1.add(ellipse1)
graphic1.add(ellipse2)
# Here
graphic = CompositeGraphic()
graphic.add(graphic1)
graphic.add(graphic2)
graphic.render()
```

### [Proxy Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/proxy.py)

> Proxy is used in places where you want to add functionality to a class without
> changing its interface. The main class is called `Real Subject`. A client should
> use the proxy or the real subject without any code change, so both must have the
> same interface. Logging and controlling access to the real subject are some of
> the proxy pattern usages.

```python
class Subject:
    """
    As mentioned in the document, interfaces of both RealSubject and Proxy should
    be the same, because the client should be able to use RealSubject or Proxy with
    no code change.
    Not all times this interface is necessary. The point is the client should be
    able to use RealSubject or Proxy interchangeably with no change in code.
    """

    def do_the_job(self, user: str) -> None:
        raise NotImplementedError()


class RealSubject(Subject):
    """
    This is the main job doer. External services like payment gateways can be a
    good example.
    """

    def do_the_job(self, user: str) -> None:
        print(f"I am doing the job for {user}")


class Proxy(Subject):
    def __init__(self) -> None:
        self._real_subject = RealSubject()

    def do_the_job(self, user: str) -> None:
        """
        logging and controlling access are some examples of proxy usages.
        """

        print(f"[log] Doing the job for {user} is requested.")

        if user == "admin":
            self._real_subject.do_the_job(user)
        else:
            print("[log] I can do the job just for `admins`.")
```

### [Model-View-Controller Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/mvc.py)

> Separates data in GUIs from the ways it is presented, and accepted.

### [Bridge Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/bridge.py)

> Decouples an abstraction from its implementation.

### [Flyweight Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/flyweight.py)

> This pattern aims to minimise the number of objects that are needed by
> a program at run-time. A Flyweight is an object shared by multiple
> contexts, and is indistinguishable from an object that is not shared.

## Creational

### [Factory Pattern](https://github.com/faif/python-patterns/blob/master/patterns/creational/factory.py)

> A Factory is an object for creating other objects.

- Depend upon abstractions. Do not depend upon concrete classes.

```python
class GreekLocalizer:
    def __init__(self) -> None:
        self.translations = {"dog": "σκύλος", "cat": "γάτα"}

    def localize(self, msg: str) -> str:
        return self.translations.get(msg, msg)


class EnglishLocalizer:
    def localize(self, msg: str) -> str:
        return msg


def get_localizer(language: str = "English") -> object:
    localizers = {
        "English": EnglishLocalizer,
        "Greek": GreekLocalizer,
    }

    return localizers[language]()
```

### [Abstract Factory Pattern](https://github.com/faif/python-patterns/blob/master/patterns/creational/abstract_factory.py)

> In Java and other languages, the Abstract Factory Pattern serves to provide an interface for
> creating related/dependent objects without need to specify their
> actual class.

```python
class PetShop:

    """A pet shop"""

    def __init__(self, animal_factory: Type[Pet]) -> None:
        """pet_factory is our abstract factory.  We can set it at will."""

        self.pet_factory = animal_factory

    def buy_pet(self, name: str) -> Pet:
        """Creates and shows a pet using the abstract factory"""

        pet = self.pet_factory(name)
        print(f"Here is your lovely {pet}")
        return pet

# Create a random animal
def random_animal(name: str) -> Pet:
    """Let's be dynamic!"""
    return random.choice([Dog, Cat])(name)
```

### [Singleton Pattern](https://github.com/faif/python-patterns/blob/master/patterns/creational/borg.py)

> Ensure a class only has one instance, and provide a global point of access to it.

In python modules are only imported once. Just declare a variable there.

### [Builder Pattern](https://github.com/faif/python-patterns/blob/master/patterns/creational/builder.py)

> It decouples the creation of a complex object and its representation,
> so that the same process can be reused to build objects from the same
> family.
> This is useful when you must separate the specification of an object
> from its actual representation (generally for abstraction).

### [Prototype Pattern](https://github.com/faif/python-patterns/blob/master/patterns/creational/prototype.py)

> This patterns aims to reduce the number of classes required by an
> application. Instead of relying on subclasses it creates objects by
> copying a prototypical instance at run-time.
