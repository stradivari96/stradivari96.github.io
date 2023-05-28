---
title: "📝 Design Patterns notes"
date: 2021-08-28
draft: false

tags: ["Software Engineering", "Programming", "Python"]
cover:
  image: "https://images.pexels.com/photos/196644/pexels-photo-196644.jpeg?cs=srgb&dl=pexels-picjumbocom-196644.jpg&fm=jpg"
---

Some notes about design patterns

<!--more-->

{{< notice info >}}

Do **NOT** use design patterns just because you can.

Use them when they make sense, they should emerge naturally from the problem.

In some languages, they might be considered anti-patterns.
{{< /notice >}}

## References
- https://refactoring.guru/design-patterns/catalog
- https://www.oreilly.com/library/view/head-first-design/9781492077992/
- https://github.com/faif/python-patterns
- https://www.patterns.dev/posts

## Behavioral

### ⭐[Strategy Pattern](https://refactoring.guru/design-patterns/strategy)

- Define a family of algorithms, encapsulate each one, and make them interchangeable.
- Enables algorithm at runtime, hide implementation details, easy to add new strategies.
- <details>
    <summary>Example</summary>

    ```python
    from typing import Protocol

    class Order:
        def __init__(
            self,
            price: float,
            discount_strategy: "DiscountStrategy" = None,
        ):
            self.price = price
            self.discount_strategy = discount_strategy

        def apply_discount(self) -> float:
            if self.discount_strategy:
                discount = self.discount_strategy(self)
            else:
                discount = 0
            return self.price - discount


    class DiscountStrategy(Protocol):
        def __call__(self, order: Order) -> float:
            ...


    def ten_percent_discount(order: Order) -> float:
        return order.price * 0.1


    def on_sale_discount(order: Order) -> float:
        return order.price * 0.25 + 20


    if __name__ == "__main__":
        order = Order(100, discount_strategy=ten_percent_discount)
        print(order.apply_discount())

        order = Order(100, discount_strategy=on_sale_discount)
        print(order.apply_discount())

    ```

    </details>

### ⭐[Observer Pattern](https://refactoring.guru/design-patterns/observer)


- Maintains a list of dependents and notifies them of any state changes.
- [Django signals](https://docs.djangoproject.com/en/4.2/topics/signals/) / [Flask signals](https://flask.palletsprojects.com/en/2.3.x/signals/)
- <details>
    <summary>Example</summary>

    ```python
    class Observer(Protocol):
        def update(self, subject: "Subject") -> None:
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


    class Data(Subject):
        def __init__(self, name: str = "") -> None:
            super().__init__()
            self.name = name
            self._data = 0

        @property
        def data(self) -> int:
            return self._data

        @data.setter
        def data(self, value: int) -> None:
            self._data = value
            self.notify()


    class HexViewer:
        def update(self, subject: Data) -> None:
            print(f"HexViewer: Subject {subject.name} has data 0x{subject.data:x}")


    class DecimalViewer:
        def update(self, subject: Data) -> None:
            print(f"DecimalViewer: Subject {subject.name} has data {subject.data}")


    if __name__ == "__main__":
        data1 = Data("Data 1")
        view1 = DecimalViewer()
        view2 = HexViewer()
        data1.attach(view1)
        data1.attach(view2)
        data1.data = 10
    ```

    </details>

### ⭐[Command Pattern](https://refactoring.guru/design-patterns/command)

- Object oriented implementation of callback functions.
- <details>
    <summary>Example</summary>

    ```python
    from typing import Protocol


    class HideFileCommand:
        def __init__(self) -> None:
            self._hidden_files: list[str] = []

        def execute(self, filename: str) -> None:
            print(f"hiding {filename}")
            self._hidden_files.append(filename)

        def undo(self) -> None:
            filename = self._hidden_files.pop()
            print(f"un-hiding {filename}")


    class DeleteFileCommand:
        def __init__(self) -> None:
            self._deleted_files: list[str] = []

        def execute(self, filename: str) -> None:
            print(f"deleting {filename}")
            self._deleted_files.append(filename)

        def undo(self) -> None:
            filename = self._deleted_files.pop()
            print(f"restoring {filename}")


    class Command(Protocol):
        def execute(self, filename: str) -> None:
            ...

        def undo(self) -> None:
            ...


    class MenuItem:
        def __init__(self, command: Command) -> None:
            self._command = command

        def on_do_press(self, filename: str) -> None:
            self._command.execute(filename)

        def on_undo_press(self) -> None:
            self._command.undo()


    if __name__ == "__main__":
        item1 = MenuItem(DeleteFileCommand())
        item2 = MenuItem(HideFileCommand())

        test_file_name = "test-file"
        item1.on_do_press(test_file_name)
        item1.on_undo_press()
        item2.on_do_press(test_file_name)
        item2.on_undo_press()
    ```

    </details>

### ⭐[Iterator Pattern](https://refactoring.guru/design-patterns/iterator)

- Provides a way to access the elements of an aggregate object
sequentially without exposing its underlying representation.

- <details>
    <summary>Example</summary>

    ```python
    def count_to(count: int):
        """Counts by word numbers, up to a maximum of five"""
        numbers = ["one", "two", "three", "four", "five"]
        yield from numbers[:count]

    def count_to_five() -> None:
        return count_to(5)

    for number in count_to_five():
        print(number)
    ```

    </details>

### [Template Method Pattern](https://refactoring.guru/design-patterns/template-method)

- Skeleton of a base algorithm, definition of exact steps in subclasses.
- [Django Class Based Views](https://docs.djangoproject.com/en/2.1/topics/class-based-views/)
- <details>
    <summary>Example</summary>

    ```python
        def template_function(getter, converter=None, to_save=False) -> None:
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

    </details>

### [State Pattern](https://refactoring.guru/design-patterns/state)

- (https://github.com/faif/python-patterns/blob/master/patterns/behavioral/state.py)
- Lets an object alter its behavior when its internal state changes.

### [Chain of Responsibility Pattern](https://refactoring.guru/design-patterns/chain-of-responsibility)

- (https://github.com/faif/python-patterns/blob/master/patterns/behavioral/chain_of_responsibility.py)
- Allow a request to pass down a chain of receivers until it is handled.

### [Mediator Pattern](https://refactoring.guru/design-patterns/mediator)

- (https://github.com/faif/python-patterns/blob/master/patterns/behavioral/mediator.py)
- Encapsulates how a set of objects interact.

### [Memento Pattern](https://refactoring.guru/design-patterns/memento)

- (https://github.com/faif/python-patterns/blob/master/patterns/behavioral/memento.py)
- Provides the ability to restore an object to its previous state.

### [Visitor Pattern](https://refactoring.guru/design-patterns/visitor)

- (https://github.com/faif/python-patterns/blob/master/patterns/behavioral/visitor.py)
- Separates an algorithm from an object structure on which it operates.

## Structural

### ⭐[Adapter Pattern](https://refactoring.guru/design-patterns/adapter)

- Allows the interface of an existing class to be used as another interface.
- <details>
    <summary>Example</summary>

    ```python
    T = TypeVar("T")

    class Adapter:

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

    cat = Cat()
    objects.append(Adapter(cat, make_noise=cat.meow))
    human = Human()
    objects.append(Adapter(human, make_noise=human.speak))
    car = Car()
    objects.append(Adapter(car, make_noise=lambda: car.make_noise(3)))

    for obj in objects:
        print("A {0} goes {1}".format(obj.name, obj.make_noise()))
    ```

    </details>

### [Decorator Pattern](https://refactoring.guru/design-patterns/decorator)

- (https://github.com/faif/python-patterns/blob/master/patterns/structural/decorator.py)
- Adds behaviour to object without affecting its class.

### [Facade Pattern](https://refactoring.guru/design-patterns/facade)

- (https://github.com/faif/python-patterns/blob/master/patterns/structural/facade.py)
- Provides a simpler unified interface to a complex system.


### [Composite Pattern](https://refactoring.guru/design-patterns/composite)

- (https://github.com/faif/python-patterns/blob/master/patterns/structural/composite.py)
- Describes a group of objects that is treated as a single instance.

### [Proxy Pattern](https://refactoring.guru/design-patterns/proxy)

- (https://github.com/faif/python-patterns/blob/master/patterns/structural/proxy.py)
- Add functionality or logic to a resource without changing its interface.

### [Model-View-Controller Pattern](https://github.com/faif/python-patterns/blob/master/patterns/structural/mvc.py)

- Separates data in GUIs from the ways it is presented, and accepted.

### [Bridge Pattern](https://refactoring.guru/design-patterns/bridge)

- (https://github.com/faif/python-patterns/blob/master/patterns/structural/bridge.py)
- Decouples an abstraction from its implementation.

### [Flyweight Pattern](https://refactoring.guru/design-patterns/flyweight)

- (https://github.com/faif/python-patterns/blob/master/patterns/structural/flyweight.py)
- Minimizes memory usage by sharing data with other similar objects.

## Creational

### ⭐[Factory Pattern](https://refactoring.guru/design-patterns/factory-method)

- Object for creating other objects without having to specify the exact class.
- [Django formset_factory](https://docs.djangoproject.com/en/4.0/topics/forms/formsets/)
- <details>
    <summary>Example</summary>

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

    e, g = get_localizer(language="English"), get_localizer(language="Greek")
    for msg in ["dog", "parrot", "cat", "bear"]:
        print(e.localize(msg), g.localize(msg))
    ```
    </details>


### ⭐[Abstract Factory Pattern](https://refactoring.guru/design-patterns/abstract-factory)

- Provides a way to encapsulate a group of individual factories.
- <details>
    <summary>Example</summary>

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
    
    cat_shop = PetShop(Cat)
    pet = cat_shop.buy_pet("Lucy")

    shop = PetShop(random_animal)
    pet = shop.buy_pet("Max")
    ```
    </details>

### ⭐[Builder Pattern](https://refactoring.guru/design-patterns/builder)

- Decouples the creation of a complex object and its representation.
- <details>
    <summary>Example</summary>

    ```python
    class Building:
        def __init__(self) -> None:
            self.build_floor()
            self.build_size()

        def build_floor(self):
            raise NotImplementedError

        def build_size(self):
            raise NotImplementedError

        def __repr__(self) -> str:
            return "Floor: {0.floor} | Size: {0.size}".format(self)


    # Concrete Buildings
    class House(Building):
        def build_floor(self) -> None:
            self.floor = "One"

        def build_size(self) -> None:
            self.size = "Big"


    class Flat(Building):
        def build_floor(self) -> None:
            self.floor = "More than One"

        def build_size(self) -> None:
            self.size = "Small"
    ```
    </details>

### [Singleton Pattern](https://refactoring.guru/design-patterns/singleton)

- (https://github.com/faif/python-patterns/blob/master/patterns/creational/borg.py)
- Ensure a class only has one instance, and provide a global point of access to it.
-In python modules are only imported once. Just declare a variable there.



### [Prototype Pattern](https://refactoring.guru/design-patterns/prototype)

- (https://github.com/faif/python-patterns/blob/master/patterns/creational/prototype.py)
- Creates new object instances by cloning prototype.
