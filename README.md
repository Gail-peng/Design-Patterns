# Python 代码设计模式

## 设计模式需要遵循的原则
<img width="1344" height="265" alt="image" src="https://github.com/user-attachments/assets/ef276a29-4be0-4377-8557-96a8c2e16a28" />

设计模式的七种原则通常被称为“SOLID原则”，是面向对象设计中的基本原则，能够帮助开发人员编写出更加灵活、可扩展、可维护的代码。这七个原则分别是：

1.单一职责原则（高内聚、低耦合）：一个类只负责一个职责或一个功能。

2.开闭原则（开放扩展，关闭修改）：一个类的行为应该是可扩展的，但是不可修改。

3.里氏替换原则（面向对象的继承和多态）：子类应该可以替换其父类并且不会影响程序的正确性。

4.接口隔离原则（减少不同功能在接口上的耦合）：一个类不应该依赖它不需要的接口，即一个类对其它类的依赖应该建立在最小的接口上。

5.依赖倒置原则（依赖抽象）：依赖于抽象而不是依赖于具体实现。这个原则强调的是代码的可扩展性和可维护性，通过抽象化来减少组件之间的耦合性，从而使得代码更加灵活、易于维护和扩展。

6.迪米特法则（对象之间的解耦）：也叫最少知识原则，一个对象应当对其他对象有尽可能少的了解，不需要了解的内容尽量不要去了解。

7.组合/聚合复用原则（使用组合等方法实现代码复用）：尽量使用组合或聚合关系，而不是继承关系来达到代码复用的目的。


## 创建型模式

创建型模式主要关注于处理**对象的创建问题**


### 1、工厂模式（Factory Method）

工厂方法模式定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。可分为简单工厂模式、工厂方法模式。以下分别对两种模式进行介绍。

【1】简单工厂模式（不属于GOF设计模式之一）

简单工厂模式属于创建型模式，又叫做静态工厂方法（Static Factory Method）。简单工厂模式是由一个工厂对象决定创建哪一种产品类实例。在简单工厂模式中，可以根据参数的不同返回不同类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。简单工厂模式是工厂模式家族中最简单实用的模式，可以理解为不同工厂模式的一个特殊实现。

值得注意的是，简单工厂模式并不属于GOF设计模式之一。但是他说抽象工厂模式，工厂方法模式的基础，并且有广泛得应用。

在简单工厂模式中，有一个工厂类负责创建多个不同类型的对象。该工厂类通常包含一个公共的静态方法，该方法接受一个参数，用于指示要创建的对象类型，然后根据该参数创建相应的对象并返回给客户端。

简单工厂模式可以隐藏对象创建的复杂性，并使客户端代码更加简洁和易于维护。但它也有一些缺点，例如如果需要添加新的对象类型，则必须修改工厂类的代码。同时，该模式也可能破坏了单一职责原则，因为工厂类不仅负责对象的创建，还负责了判断要创建哪个对象的逻辑。

简单工厂模式通常被用于创建具有相似特征的对象，例如不同类型的图形对象、不同类型的数据库连接对象等。

下面是一个简单工厂模式的 Python 实现示例：

```
class Product:
    def operation(self):
        pass

class ConcreteProductA(Product):
    def operation(self):
        return "ConcreteProductA"

class ConcreteProductB(Product):
    def operation(self):
        return "ConcreteProductB"

# 此处设计的类根据输入的参数不同，生成不同的产品对象
class SimpleFactory:
    @staticmethod
    def create_product(product_type): # 静态方法为类的方法，不需要实例化，此处没有传入self参数。
        if product_type == "A":
            return ConcreteProductA()
        elif product_type == "B":
            return ConcreteProductB()
        else:
            raise ValueError("Invalid product type")

if __name__ == "__main__":
    # 客户端代码
    product_a = SimpleFactory.create_product("A")
    product_b = SimpleFactory.create_product("B")

    print(product_a.operation())  # 输出：ConcreteProductA
    print(product_b.operation())  # 输出：ConcreteProductB
'''
在这段代码中，create_product 方法被 @staticmethod 装饰器标记为静态方法，这是不需要通过 SimpleFactory() 创建实例就能调用该方法的核心原因。

静态方法（@staticmethod）是属于类本身的方法，而非类的实例。它不需要依赖类的实例状态，也不强制要求传入 self 参数。因此，调用静态方法时无需创建类的实例，直接通过「类名。方法名」的方式即可调用。

简单工厂模式中，工厂类的核心作用是封装对象的创建逻辑，本身通常不需要维护实例状态
'''
```

【2】工厂方法模式

工厂方法模式（Factory Method）是一种创建型设计模式，它提供了一种将对象的创建过程委托给子类的方式。

通常情况下，工厂方法模式使用一个接口或抽象类来表示创建对象的工厂，然后将具体的创建逻辑委托给子类来实现。这样可以使代码更加灵活，因为在运行时可以选择要实例化的对象类型。

以下是工厂方法模式的基本原理：

1.定义一个接口或抽象类来表示要创建的对象。
2.创建一个工厂类，该类包含一个工厂方法，该方法根据需要创建对象并返回该对象。
3.创建一个或多个具体的子类，实现工厂接口并实现工厂方法来创建对象。

工厂模式的优点：

这种方法可以使代码更加灵活，因为在运行时可以选择要实例化的对象类型。例如，可以轻松地添加新的子类来创建不同的对象类型，而无需更改现有的代码。

工厂方法模式还提供了一种解耦的方式，因为它将对象的创建逻辑与其使用代码分离。这可以使代码更加可维护和可测试，因为可以独立地测试和修改对象的创建逻辑和使用代码。

工厂方法模式常用于框架和库中，因为它可以使用户扩展框架或库的功能，而无需更改框架或库的源代码。

下面是一个在 Python 中实现工厂方法模式的例子：

```
from abc import ABC, abstractmethod

# 定义抽象产品类
class Product(ABC):
    @abstractmethod
    def use(self):
        pass

# 定义具体产品类 A
class ConcreteProductA(Product):
    def use(self):
        print("Using product A")

# 定义具体产品类 B
class ConcreteProductB(Product):
    def use(self):
        print("Using product B")

# 定义工厂类
class Creator(ABC):
    @abstractmethod
    def factory_method(self):
        pass

    def some_operation(self):
        product = self.factory_method()
        product.use()

# 定义具体工厂类 A
class ConcreteCreatorA(Creator):
    def factory_method(self):
        return ConcreteProductA()

# 定义具体工厂类 B
class ConcreteCreatorB(Creator):
    def factory_method(self):
        return ConcreteProductB()

# 客户端代码，客户端无需直接创建产品（如ConcreteProductA），而是通过创建具体工厂（如ConcreteCreatorA），调用工厂的some_operation方法间接使用产品
if __name__ == "__main__":
    creator_a = ConcreteCreatorA()
    creator_a.some_operation()

    creator_b = ConcreteCreatorB()
    creator_b.some_operation()
```
代码讲解：

在上面的例子中，我们首先定义了一个抽象产品类 Product，它包含了一个抽象方法 use，它将由具体产品类去实现。然后我们定义了两个具体产品类 ConcreteProductA 和 ConcreteProductB，它们实现了 Product 类中定义的抽象方法。

接下来，我们定义了一个抽象工厂类 Creator，它包含了一个抽象工厂方法 factory_method，这个方法将由具体工厂类去实现。我们还定义了一个 some_operation 方法，它使用工厂方法创建产品并调用 use 方法。

最后，我们定义了两个具体工厂类 ConcreteCreatorA 和 ConcreteCreatorB，它们分别实现了 Creator 类中的 factory_method 方法，返回具体产品类的实例。

在客户端代码中，我们首先创建一个 ConcreteCreatorA 对象，并调用 some_operation 方法，它会使用 ConcreteCreatorA 工厂方法创建一个 ConcreteProductA 对象并调用 use 方法。然后我们创建一个 ConcreteCreatorB 对象，同样调用 some_operation 方法，它会使用 ConcreteCreatorB 工厂方法创建一个 ConcreteProductB 对象并调用 use 方法。

from abc import ABC, abstractmethod 导入的是 Python 中用于实现抽象基类（Abstract Base Class, ABC） 的工具，它们的核心作用是强制类的结构规范，确保子类遵循父类定义的接口。


ABC的作用：

ABC是所有抽象基类的基类（父类）。一个类如果继承自ABC，则成为抽象基类，它的特点是：

1.不能被直接实例化（例如Product()或Creator()会报错），只能***作为父类被继承***。

2.用于定义 “接口规范”，***规定子类必须实现哪些方法***。


@abstractmethod的作用：

@abstractmethod是一个装饰器，用于***标记抽象基类中的抽象方法***。抽象方法的特点是：

1.只有***方法定义（函数名、参数）***，没有具体实现（没有函数体，或用pass占位）。

2.***强制子类必须重写该方法***：如果子类继承了抽象基类但没有实现所有抽象方法，那么子类也会被视为抽象基类，无法实例化。


## 2、抽象工厂模式（AbstractFactory）

抽象工厂模式（Abstract Factory）是一种创建型设计模式，它提供一个接口，用于创建一系列相关或相互依赖的对象，而不需要指定它们的具体类。

抽象工厂模式的主要目的是封装一组具有相似主题的工厂，使客户端能够以一种与产品实现无关的方式创建一组相关的产品。抽象工厂模式提供了一个抽象工厂类和一组抽象产品类，具体工厂和具体产品类由它们的子类来实现。

下面是抽象工厂模式的 UML 类图：

<img width="548" height="393" alt="image" src="https://github.com/user-attachments/assets/27a2a2d1-a80c-4168-b139-cf7e4c2c243b" />

其中，AbstractFactory 是抽象工厂类，它定义了一组用于创建产品的抽象方法；ConcreteFactory1 和 ConcreteFactory2 是具体工厂类，它们分别实现了抽象工厂类中定义的抽象方法，用于创建一组具体产品；ProductA1、ProductA2、ProductB1 和 ProductB2 是抽象产品类，它们定义了一组用于产品的抽象方法。

下面是一个使用抽象工厂模式的例子，假设我们要开发一个跨平台的图形用户界面，包括按钮、文本框和选择框等控件，我们可以先定义一组抽象控件类和抽象工厂类：

```
from abc import ABC, abstractmethod

class Button(ABC):
    @abstractmethod
    def paint(self):
        pass

class TextBox(ABC):
    @abstractmethod
    def paint(self):
        pass

class CheckBox(ABC):
    @abstractmethod
    def paint(self):
        pass

class GUIFactory(ABC):
    @abstractmethod
    def create_button(self) -> Button:
        pass

    @abstractmethod
    def create_text_box(self) -> TextBox:
        pass

    @abstractmethod
    def create_check_box(self) -> CheckBox:
        pass
```


之后，定义一组具体的控件类和工厂类，比如 Windows 控件和 Mac 控件：
```
class WindowsButton(Button):
    def paint(self):
        print("Painting a Windows style button")

class WindowsTextBox(TextBox):
    def paint(self):
        print("Painting a Windows style text box")

class WindowsCheckBox(CheckBox):
    def paint(self):
        print("Painting a Windows style check box")

class WindowsFactory(GUIFactory):
    def create_button(self) -> Button:
        return WindowsButton()

    def create_text_box(self) -> TextBox:
        return WindowsTextBox()

    def create_check_box(self) -> CheckBox:
        return WindowsCheckBox()


class MacButton(Button):
    def paint(self):
        print("Painting a Mac style button")

class MacTextBox(TextBox):
    def paint(self):
        print("Painting a Mac style text box")

class MacCheckBox(CheckBox):
    def paint(self):
        print("Painting a Mac style check box")

class MacFactory(GUIFactory):
    def create_button(self) -> Button:
        return MacButton()

    def create_text_box(self) -> TextBox:
        return MacTextBox()

    def create_check_box(self) -> CheckBox:
        return MacCheckBox()


def create_gui(factory: GUIFactory):
    button = factory.create_button()
    text_box = factory.create_text_box()
    check_box = factory.create_check_box()
    return button, text_box, check_box

windows_gui = create_gui(WindowsFactory())
mac_gui = create_gui(MacFactory())
```

代码讲解：

在这个例子中，抽象工厂类 GUIFactory 定义了一组用于创建控件的抽象方法，具体工厂类 WindowsFactory 和 MacFactory 分别实现了这些方法来创建具有不同样式的 Windows 和 Mac 控件。

客户端代码使用不同的工厂类来创建具有不同样式的 GUI，但是它并不知道具体创建了哪些控件类。这就实现了客户端与产品实现之间的解耦。


## 3、单例模式（Singleton）

单例模式（Singleton）是一种创建型设计模式，其原理是确保一个类只有一个实例，并且提供了一个访问该实例的全局点。

单例模式可以使用多种不同的实现方式，但它们的基本原理是相同的。通常，单例模式使用一个私有构造函数来确保只有一个对象被创建。然后，它提供了一个全局点访问该对象的方法，使得任何代码都可以访问该对象，而不必担心创建多个实例。

具体来说，单例模式通常通过以下几个步骤实现：

1.创建一个私有构造函数，以确保类不能从外部实例化。
2.创建一个私有静态变量，用于存储类的唯一实例。
3.创建一个公共静态方法，用于访问该实例。
在公共静态方法中，如果实例不存在，则创建一个新实例并将其分配给静态变量。否则，返回现有的实例。

优缺点：

单例模式可以有效地避免重复的内存分配，特别是当对象需要被频繁地创建和销毁时。另外，单例模式还提供了一种简单的方式来控制全局状态，因为只有一个实例存在，可以确保任何代码都在同一个对象上运行。

然而，单例模式可能导致线程安全问题。如果多个线程同时尝试访问单例实例，可能会导致竞争条件。因此，在实现单例模式时需要格外小心，并考虑到线程安全问题。

以下是一个简单的Python示例，实现了单例模式的基本原理：

```
class Singleton:
    __instance = None

    def __init__(self):
        if Singleton.__instance is not None:
            raise Exception("This class is a singleton!")
        else:
            Singleton.__instance = self

    @staticmethod  #此处的静态方法同上
    def get_instance():
        if Singleton.__instance is None:
            Singleton()
        return Singleton.__instance

if __name__ == "__main__":
    s1 = Singleton.get_instance()
    s2 = Singleton.get_instance()

    print(s1 is s2)  # True
```
实现思路：

在上面的示例中，我们创建了一个名为Singleton的类，并使用__init__()方法确保只有一个实例。在__init__()方法中，我们首先检查是否已经有一个实例存在。如果是这样，我们引发一个异常，否则我们将当前实例分配给__instance属性。

接下来，我们创建了一个名为get_instance()的公共静态方法，以便访问该实例。在get_instance()方法中，我们首先检查是否已经有一个实例存在。如果没有，我们将创建一个新实例，并将其分配给__instance属性。否则，我们将返回现有的实例。

这种方法的主要优点是，只有一个实例被创建，可以避免重复的内存分配。另外，它提供了一个全局点访问该实例。


## 4、建造者模式（Builder）

建造者模式（Builder）是一种创建型设计模式，它允许我们按照特定顺序组装一个复杂的对象。建造者模式将对象的构造过程分解为多个步骤，每个步骤都由一个具体的构造者来完成。这样，客户端可以根据需要使用不同的构造者来构建不同的对象，而不必知道构造过程的具体细节。

在 Python 中，建造者模式通常使用构造者类来封装对象的构造过程，以及指导客户端如何构建对象。

具体的构造者类可以继承自一个抽象的构造者类，并实现其定义的构造方法，从而实现具体的构造过程。

客户端可以选择任何一个具体构造者类来构建对象，并且也可以自定义构造者类来实现自定义的构造过程。

下面是一个简单的 Python 实现，演示了如何使用建造者模式来构建一个包含多个部件的复杂对象（汽车）：

```
from abc import ABC, abstractmethod

class CarBuilder(ABC):
    @abstractmethod
    def reset(self):
        pass

    @abstractmethod
    def set_seats(self, number_of_seats):
        pass

    @abstractmethod
    def set_engine(self, engine_power):
        pass

    @abstractmethod
    def set_trip_computer(self):
        pass

    @abstractmethod
    def set_gps(self):
        pass

class Car:
    def __init__(self):
        self.seats = 0
        self.engine_power = 0
        self.trip_computer = False
        self.gps = False

    def __str__(self):
        return f'Car with {self.seats} seats, {self.engine_power} engine, trip computer: {self.trip_computer}, GPS: {self.gps}'

class SportsCarBuilder(CarBuilder):
    def __init__(self):
        self.car = Car()

    def reset(self):
        self.car = Car()

    def set_seats(self, number_of_seats):
        self.car.seats = number_of_seats

    def set_engine(self, engine_power):
        self.car.engine_power = engine_power

    def set_trip_computer(self):
        self.car.trip_computer = True

    def set_gps(self):
        self.car.gps = True

    def get_car(self):
        return self.car

class Director:
    def __init__(self, builder):
        self.builder = builder

    def build_sports_car(self):
        self.builder.reset()
        self.builder.set_seats(2)
        self.builder.set_engine(300)
        self.builder.set_trip_computer()
        self.builder.set_gps()
        return self.builder.get_car()

if __name__ == '__main__':
    sports_car_builder = SportsCarBuilder()
    director = Director(sports_car_builder)
    sports_car = director.build_sports_car()
    print(sports_car)
```

代码讲解：

在这个例子中，我们定义了一个抽象的 CarBuilder 类，它有五个构造方法，分别用于重置汽车、设置座位数量、设置引擎功率、安装行车电脑和安装 GPS。

SportsCarBuilder 类继承自 CarBuilder，实现了这些构造方法，并定义了一个 get_car() 方法，用于获取构建好的汽车对象。

Director 类则用来指导汽车的构建过程，它接收一个具体的构造者对象，并根据需要的顺序调用构造方法来构建汽车。

在主函数中，我们先创建一个 SportsCarBuilder 对象，然后使用它来构建一辆跑车。

构建过程由 Director 对象来指导，并返回构建好的汽车对象。最后，我们打印这辆汽车的信息，即它的座位数量、引擎功率、是否安装行车电脑和是否安装 GPS。

需要注意的是，在实际应用中，建造者模式常常会和其他设计模式一起使用，比如工厂方法模式和单例模式。此外，建造者模式还常常被用于构建复杂的 DOM 结构和 XML 文档。


## 5、原型模式（Prototype）

原型模式（Prototype）是一种创建型设计模式，它允许通过复制现有对象来创建新对象，而不是通过实例化类来创建对象。原型模式允许我们创建一个原型对象，然后通过克隆这个原型对象来创建新的对象，从而避免了重复的初始化操作。

在 Python 中，可以使用 copy 模块中的 copy() 和 deepcopy() 函数来实现原型模式。

copy() 函数执行的是浅复制，它复制对象本身，但不复制对象引用的内存空间，因此如果原型对象中包含可变对象（如列表、字典等），那么新对象和原型对象将共享这些可变对象。

deepcopy() 函数则执行深复制，它会递归地复制对象及其引用的所有对象，因此新对象和原型对象不会共享任何对象。

下面是一个简单的 Python 实现，演示了如何使用原型模式创建和克隆一个包含可变和不可变成员的对象：

```
import copy

class Prototype:
    def __init__(self, x, y, items):
        self.x = x
        self.y = y
        self.items = items

    def clone(self):
        return copy.deepcopy(self)

if __name__ == '__main__':
    items = ['item1', 'item2', 'item3']
    original = Prototype(1, 2, items)
    clone = original.clone()

    print(f'Original: x={original.x}, y={original.y}, items={original.items}')
    print(f'Clone: x={clone.x}, y={clone.y}, items={clone.items}')

    items.append('item4')
    original.x = 5
    original.y = 10

    print(f'Original (updated): x={original.x}, y={original.y}, items={original.items}')
    print(f'Clone (not updated): x={clone.x}, y={clone.y}, items={clone.items}')
```
代码讲解：

在这个例子中，Prototype 类有三个成员：x、y 和 items。

clone() 方法使用深度复制来复制对象及其所有成员。

客户端代码首先创建一个原型对象，然后克隆它以创建一个新对象。

接下来，客户端代码更新原型对象的成员，但是新对象不会受到影响，因为它们共享的是不同的内存空间。


## 结构型模式

关注**对象和类之间的组合**，实现对象**组合过程中的影响解耦**，使得单个对象或类修改不会影响整体组合的代码


### 1、适配器模式（Adapter）

适配器模式（Adapter）是一种结构型设计模式，用于将一个类的接口转换为另一个类的接口。适配器模式的作用是解决两个不兼容的接口之间的兼容问题，从而使它们能够协同工作。

适配器模式由三个主要组件组成：

目标接口（Target Interface）：是客户端代码期望的接口。在适配器模式中，它通常由抽象类或接口表示。

适配器（Adapter）：是实现目标接口的对象。适配器通过包装一个需要适配的对象，并实现目标接口来实现适配的效果。

源接口（Adaptee Interface）：是需要被适配的接口。在适配器模式中，它通常由一个或多个具体类或接口表示。

适配器模式通常有两种实现方式：

类适配器模式：通过继承来实现适配器，从而使适配器成为源接口的子类，并实现目标接口。这种方式需要适配器能够覆盖源接口的所有方法。

对象适配器模式：通过组合来实现适配器，从而使适配器持有一个源接口的对象，并实现目标接口。这种方式可以在适配器中自定义需要适配的方法，而无需覆盖源接口的所有方法。

优缺点：

适配器模式的优点是能够解决两个不兼容接口之间的兼容问题，并且可以使代码更加灵活和可扩展。

它的缺点是需要额外的适配器对象，可能会导致代码的复杂性增加。在设计过程中，需要根据具体的场景和需求，选择最合适的适配器实现方式。

下面是一个类适配器模式的 UML 类图：

<img width="815" height="505" alt="image" src="https://github.com/user-attachments/assets/e0f6b781-6749-4b66-b1c6-6faeb656fd55" />

```
# 目标接口
class Target:
    def request(self):
        pass

# 源接口
class Adaptee:
    def specific_request(self):
        pass

# 类适配器
class Adapter(Target, Adaptee):
    def request(self):
        self.specific_request()
        # 其他逻辑

# 对象适配器
class Adapter2(Target):
    def __init__(self, adaptee):
        self._adaptee = adaptee

    def request(self):
        self._adaptee.specific_request()
        # 其他逻辑

# 客户端代码
def client_code(target):
    target.request()

adaptee = Adaptee()
adapter = Adapter()
adapter2 = Adapter2(adaptee)

client_code(adapter)
client_code(adapter2
```
代码解释：

在上面的代码中，我们首先定义了目标接口 Target 和源接口 Adaptee，然后实现了两种适配器：类适配器 Adapter 和对象适配器 Adapter2。

在类适配器中，我们使用多重继承来同时继承目标接口和源接口，并实现目标接口的 request 方法。在这个方法中，我们调用源接口的 specific_request 方法，并在必要的情况下进行其他逻辑处理。

在对象适配器中，我们使用组合来持有一个源接口的对象，并实现目标接口的 request 方法。在这个方法中，我们调用源接口的 specific_request 方法，并在必要的情况下进行其他逻辑处理。

最后，我们定义了一个客户端代码 client_code，它接收一个目标接口的实例作为参数，并调用该实例的 request 方法。我们分别用类适配器和对象适配器来适配源接口，并将适配器传递给客户端代码进行测试。


### 2、桥接模式（Bridge）

桥接模式（Bridge）是一种结构型设计模式，旨在将抽象部分和具体实现部分分离，使它们可以独立地变化。

桥接模式的原理实现基于面向对象的多态特性，其核心思想是将抽象部分和实现部分解耦，使得它们可以独立地变化而互不影响。在桥接模式中，抽象部分和实现部分分别由抽象类和实现类来表示，它们之间通过一个桥梁接口来联系。

具体的实现步骤如下：

定义抽象类和实现类：抽象类定义了抽象部分的接口，包含了一些基本的方法。实现类定义了实现部分的接口，包含了一些实现方法。

定义桥梁接口：桥梁接口定义了抽象部分和实现部分之间的连接，它包含了一个对实现类的引用，以及一些委托方法。

定义具体桥梁类：具体桥梁类继承了桥梁接口，实现了委托方法，将调用转发给实现类的方法。

实例化具体桥梁类：在程序运行时，实例化具体桥梁类，并将实现类对象作为参数传递给具体桥梁类的构造函数。

调用具体桥梁类的方法：在程序运行时，调用具体桥梁类的方法，具体桥梁类将委托给实现类的方法来完成具体的操作。

下面是一个 Python 示例代码，演示了如何使用桥接模式实现不同形状的颜色填充：

```
# 抽象类：形状
class Shape:
    def __init__(self, color):
        self.color = color

    def draw(self):
        pass

# 实现类：颜色
class Color:
    def fill(self):
        pass

# 实现类的具体实现：红色
class RedColor(Color):
    def fill(self):
        return "Red"

# 实现类的具体实现：绿色
class GreenColor(Color):
    def fill(self):
        return "Green"

# 桥梁接口
class Bridge:
    def __init__(self, color):
        self.color = color

    def draw(self):
        pass

# 具体桥梁类：圆形
class Circle(Bridge):
    def draw(self):
        return "Circle filled with " + self.color.fill()

# 具体桥梁类：矩形
class Rectangle(Bridge):
    def draw(self):
        return "Rectangle filled with " + self.color.fill()

# 使用示例
red = RedColor()
green = GreenColor()
circle = Circle(red)
rectangle = Rectangle(green)
print(circle.draw())      # 输出：Circle filled with Red
print(rectangle.draw())   # 输出：Rectangle filled with Green
```

代码讲解：

在这个示例中，Shape是抽象类，它包含了一个颜色属性和draw方法。Color是实现类，它包含了一个fill方法。RedColor和GreenColor是Color的具体实现类，它们分别实现了fill方法返回红色和绿色。

Bridge是桥梁接口，它包含了一个对Color的引用。Circle和Rectangle是具体桥梁类，它们继承了Bridge，实现了draw方法，将调用转发给实现类的fill方法。

在示例的最后，实例化了RedColor和GreenColor，并分别传递给Circle和Rectangle作为参数。然后调用了draw方法，输出了Circle和Rectangle的颜色填充。

通过桥接模式，将抽象部分和实现部分分离开来，可以使得它们可以独立地变化而互不影响。这样就可以更加灵活地组合不同的抽象部分和实现部分，从而实现更加复杂的功能。


### 3、组合模式（Composite）

组合模式（Composite）是一种结构型设计模式，它允许你将对象组合成树形结构来表示整体-部分关系，使得客户端可以统一地处理单个对象和组合对象。

该模式包含以下几个角色：

抽象组件（Component）：定义了组合中所有对象共有的行为，并规定了管理子组件的方法。

叶子组件（Leaf）：表示组合中的单个对象，叶子节点没有子节点。

容器组件（Composite）：表示组合中的容器对象，容器节点可以包含其他容器节点和叶子节点。

客户端（Client）：通过抽象组件操作组合对象。

优点：

使用组合模式可以使得客户端可以像处理单个对象一样处理组合对象。

同时也方便了新增或删除子组件。

该模式通常应用于处理树形结构数据或者嵌套的对象结构。

以下是一个简单的组合模式的示例，假设我们要处理一些文件和文件夹，文件夹可以包含其他文件和文件夹，我们可以使用组合模式来表示它们之间的整体-部分关系：

```
from abc import ABC, abstractmethod

class FileComponent(ABC):
    @abstractmethod
    def list(self):
        pass

class File(FileComponent):           # 叶子组件，表示文件
    def __init__(self, name):
        self.name = name

    def list(self):
        print(self.name)

class Folder(FileComponent):         # 容器组件，表示文件夹
    def __init__(self, name):
        self.name = name
        self.children = []

    def add(self, component):
        self.children.append(component)

    def remove(self, component):
        self.children.remove(component)

    def list(self):
        print(self.name)
        for child in self.children:
            child.list()

if __name__ == "__main__":
    root = Folder("root")
    folder1 = Folder("folder1")
    folder2 = Folder("folder2")
    file1 = File("file1")
    file2 = File("file2")
    
    root.add(folder1)
    root.add(folder2)
    root.add(file1)
    folder1.add(file2)
    
    root.list()
```

在上述示例中，FileComponent 类是抽象组件，定义了组合中所有对象共有的行为。File 类是叶子组件，表示文件。Folder 类是容器组件，表示文件夹，它可以包含其他文件和文件夹。

客户端可以通过抽象组件对文件和文件夹进行操作，在上述示例中，我们创建了一个根节点root，包含了两个文件夹folder1和folder2以及两个文件file1和file2。客户端通过root节点列出了整个文件树的结构



### 4、装饰模式（Decorator）

装饰模式（Decorator）是一种结构型设计模式，它允许你在运行时为对象动态添加功能。装饰模式是一种替代继承的方式，它通过将对象放入包装器对象中来实现这一点。这种模式是开放封闭原则的一种具体实现方式。

在装饰模式中，有一个抽象组件（Component）类，它定义了基本的操作方法。

有一个具体组件（ConcreteComponent）类，它实现了抽象组件类中定义的操作方法。

还有一个装饰器（Decorator）类，它也实现了抽象组件类中定义的操作方法，并且它包含一个指向抽象组件类的引用。

此外，还有一个具体装饰器（ConcreteDecorator）类，它扩展了装饰器类，以实现额外的功能。

装饰模式的核心思想是，在不改变原有类的情况下，通过包装原有类来扩展其功能。这使得我们可以在运行时动态地添加功能，而不需要在编译时修改代码。

以下是一个装饰模式的 UML 类图：

<img width="720" height="419" alt="image" src="https://github.com/user-attachments/assets/83eff27c-1690-4c72-bcbc-a61131ee7ff2" />

Component: 抽象组件类，定义了基本的操作方法。

Decorator: 装饰器类，实现了抽象组件类中定义的操作方法，并且它包含一个指向抽象组件类的引用。

ConcreteComponent: 具体组件类，实现了抽象组件类中定义的操作方法。

ConcreteDecorator: 具体装饰器类，扩展了装饰器类，以实现额外的功能。

温馨提示：

在装饰模式中，可以通过组合的方式来添加多个装饰器，从而实现对对象的多次装饰。

同时，装饰器对象可以嵌套在其他装饰器对象内部，从而形成一个装饰器对象的树形结构，这种结构称为装饰器链。

在执行操作时，装饰器对象会按照一定的顺序递归地调用装饰器链中的操作方法。

下面是一个装饰模式的 Python 实现示例：
```
from abc import ABC, abstractmethod

# 定义抽象组件
class Component(ABC):
    @abstractmethod
    def operation(self):
        pass

# 定义具体组件
class ConcreteComponent(Component):
    def operation(self):
        return "ConcreteComponent"

# 定义抽象装饰器
class Decorator(Component):
    def __init__(self, component: Component):
        self._component = component

    @abstractmethod
    def operation(self):
        pass

# 定义具体装饰器 A
class ConcreteDecoratorA(Decorator):
    def operation(self):
        return f"ConcreteDecoratorA({self._component.operation()})"

# 定义具体装饰器 B
class ConcreteDecoratorB(Decorator):
    def operation(self):
        return f"ConcreteDecoratorB({self._component.operation()})"

if __name__ == "__main__":
    component = ConcreteComponent()
    decoratorA = ConcreteDecoratorA(component)
    decoratorB = ConcreteDecoratorB(decoratorA)
    print(decoratorB.operation())
```
代码讲解：

在上面的实现中，我们首先定义了一个抽象组件 Component，其中包含了一个抽象方法 operation，用于定义基本操作。

接着，我们定义了具体组件 ConcreteComponent，它是 Component 的一个实现类，实现了 operation 方法。

然后，我们定义了抽象装饰器 Decorator，它也是 Component 的一个实现类，其中包含了一个指向抽象组件 Component 的引用。同时，它也是一个抽象类，其中包含了一个抽象方法 operation，用于定义装饰器的操作。

接着，我们定义了具体装饰器 A 和 B，它们都继承自 Decorator 类。在 ConcreteDecoratorA 和 ConcreteDecoratorB 中，我们重写了 operation 方法，并在其中调用了 self._component.operation() 方法，即对 Component 进行了包装，以实现对组件的功能扩展。

最后，在主函数中，我们创建了一个 ConcreteComponent 对象，并用具体装饰器 A 和 B 对它进行了多次包装，从而实现了对组件的多次功能扩展。最终，调用 decoratorB.operation() 方法时，输出了 ConcreteDecoratorB(ConcreteDecoratorA(ConcreteComponent))，即对 

ConcreteComponent 对象进行了两次包装，并返回了最终的结果。


### 5、外观模式（Facade）

外观模式（Facade）是一种结构型设计模式，它提供了一个简单的接口，隐藏了一个或多个复杂的子系统的复杂性。外观模式可以使得客户端只需要与外观对象进行交互，而不需要与子系统中的每个对象直接交互，从而降低了客户端的复杂性，提高了系统的可维护性。

实现思路：

外观模式的核心思想是，提供一个简单的接口，包装一个或多个复杂的子系统，隐藏其复杂性，并向客户端提供一个更简单、更易于使用的接口。

在外观模式中，外观对象扮演着客户端和子系统之间的协调者，它负责将客户端的请求转发给子系统中的相应对象，并将其结果返回给客户端。

外观模式的优点包括：

简化了客户端的使用：外观模式为客户端提供了一个简单的接口，使得客户端不需要了解子系统中的每个对象及其功能，从而降低了客户端的复杂性。

隐藏了子系统的复杂性：外观模式将子系统的复杂性隐藏在外观对象之后，使得客户端只需要与外观对象进行交互，从而提高了系统的可维护性。

提高了灵活性：由于客户端只与外观对象进行交互，因此可以在不影响客户端的情况下修改或替换子系统中的对象。

外观模式的缺点包括：

不能完全隐藏子系统的复杂性：外观模式只是将子系统的复杂性隐藏在外观对象之后，但仍然需要客户端了解外观对象的接口和使用方式。

可能会引入不必要的复杂性：如果外观对象需要处理复杂的逻辑，就会引入额外的复杂性，从而降低系统的可维护性。

以下是一个使用外观模式的示例，假设我们有一个音乐播放器，它可以播放MP3和FLAC两种格式的音乐。不同格式的音乐播放需要不同的解码器，同时还需要加载音乐文件和设置音量等操作。我们可以使用外观模式封装这些复杂的操作，提供一个简单易用的接口给客户端使用：

```
class MusicPlayer:
    def __init__(self):
        self.mp3_player = MP3Player()
        self.flac_player = FLACPlayer()

    def play_mp3(self, file_path, volume):
        self.mp3_player.load(file_path)
        self.mp3_player.set_volume(volume)
        self.mp3_player.play()

    def play_flac(self, file_path, volume):
        self.flac_player.load(file_path)
        self.flac_player.set_volume(volume)
        self.flac_player.play()


class MP3Player:
    def load(self, file_path):
        print(f"Loading MP3 file from {file_path}")

    def set_volume(self, volume):
        print(f"Setting MP3 volume to {volume}")

    def play(self):
        print("Playing MP3 music")


class FLACPlayer:
    def load(self, file_path):
        print(f"Loading FLAC file from {file_path}")

    def set_volume(self, volume):
        print(f"Setting FLAC volume to {volume}")

    def play(self):
        print("Playing FLAC music")
```
代码讲解：

在上述示例中，MusicPlayer 类是外观类，它封装了 MP3 和 FLAC 播放器对象。play_mp3 和 play_flac 方法是外观类中的简单接口，它们将客户端的请求转发给相应的播放器对象。

客户端只需要使用MusicPlayer类就可以进行MP3和FLAC的播放，而不需要了解播放器的具体实现。如果需要修改或替换播放器中的对象，只需要修改外观类的实现即可，而不会影响客户端的使用。


### 6、享元模式（Flyweight）

享元模式（Flyweight）是一种结构型设计模式，它通过共享对象来尽可能减少内存使用和对象数量。在享元模式中，存在两种对象：内部状态（Intrinsic State）和外部状态（Extrinsic State）。内部状态指对象的共享部分，不随环境改变而改变；外部状态指对象的非共享部分，会随环境改变而改变。

实现思路：

享元模式的核心思想是尽量重用已经存在的对象，减少对象的创建和销毁，从而提高性能和节省内存。

它通常适用于需要大量创建对象的场景，但又不能因为对象过多而导致内存不足或性能降低的情况。

下面是一个简单的享元模式的示例，假设我们有一个字符工厂，它可以创建不同的字符对象。在实现字符对象时，我们发现有一些字符会被频繁使用，而且它们的状态是不变的，例如空格、逗号、句号等标点符号。因此，我们可以将这些字符设计为享元对象，通过共享来节省内存。

```
class CharacterFactory:
    def __init__(self):
        self.characters = {}

    def get_character(self, character):
        if character in self.characters:
            return self.characters[character]
        else:
            new_character = Character(character)
            self.characters[character] = new_character
            return new_character

class Character:
    def __init__(self, character):
        self.character = character

    def render(self, font):
        print(f"Rendering character {self.character} in font {font}")

# 创建字符工厂
factory = CharacterFactory()

# 获取不同的字符
char1 = factory.get_character("A")
char2 = factory.get_character("B")
char3 = factory.get_character(" ")
char4 = factory.get_character(",")
char5 = factory.get_character(" ")
char6 = factory.get_character(".")

# 渲染不同的字符
char1.render("Arial")
char2.render("Times New Roman")
char3.render("Arial")
char4.render("Times New Roman")
char5.render("Arial")
char6.render("Times New Roman")
```
代码讲解：

在上述示例中，我们创建了一个CharacterFactory类来管理字符对象。当客户端需要获取一个字符时，可以调用get_character方法。如果该字符已经被创建过了，就直接返回共享的对象；否则，创建一个新的对象并将其保存到工厂中，以备下次使用。

字符对象Character有一个render方法，用于渲染该字符。在实际使用中，我们可能需要给不同的字符设置不同的字体，这里只是为了演示方便，用字符串代替了字体对象。

通过享元模式，我们可以共享多个相同的字符对象，从而减少内存使用和对象数量。在这个例子中，如果没有使用享元模式，我们可能需要创建多个空格、逗号和句号对象，而这些对象的状态都是不变的，这样就会导致内存浪费。通过使用享元模式，我们可以将这些相同的对象共享起来，避免重复创建

对象，从而提高性能和节省内存。


### 7、代理模式（Proxy）

代理模式（Proxy）是一种结构型设计模式，***它允许在访问对象时添加一些额外的行为***。代理类充当客户端和实际对象之间的中介。客户端通过代理来访问实际对象，代理在访问实际对象前后执行一些额外的操作，例如权限检查、缓存等。

代理模式包含三个角色：

抽象主题（Subject）：抽象主题定义了真实主题和代理主题的公共接口

真实主题（Real Subject）：真实主题是实际执行操作的对象

代理主题（Proxy Subject）：代理主题通过实现抽象主题接口，控制对真实主题的访问。

<img width="592" height="136" alt="image" src="https://github.com/user-attachments/assets/5137f1d1-d72f-4219-9f4e-68e03d90d910" />

在上面的类图中，Subject 是抽象主题，定义了客户端和真实主题之间的接口，RealSubject 是真实主题，实现了抽象主题定义的接口，ProxySubject 是代理主题，也实现了抽象主题定义的接口，并且内部持有一个 RealSubject 对象，以便在需要时代理访问 RealSubject 对象。

下面是一个 Python 实现的示例，假设我们有一个邮件服务器，我们需要实现一个邮件客户端程序，但我们不想直接连接到邮件服务器，因为这样可能会存在一些风险，我们想通过代理来连接邮件服务器，以此增加一些安全性：

```
# 抽象主题
class Email:
    def send(self, message):
        pass

# 真实主题
class EmailServer(Email):
    def send(self, message):
        print(f'Sending email: {message}')

# 代理主题
class EmailProxy(Email):
    def __init__(self, email_server):
        self.email_server = email_server
    
    def send(self, message):
        if self.is_allowed_to_send(message):
            self.email_server.send(message)
            self.log(message)
        else:
            print('Not allowed to send email')
    
    def is_allowed_to_send(self, message):
        # Check if user is allowed to send the email
        return True
    
    def log(self, message):
        # Log the email to a file
        print(f'Logging email: {message}')

# 客户端
if __name__ == '__main__':
    email_server = EmailServer()
    email_proxy = EmailProxy(email_server)
    email_proxy.send('Hello, world!')
```

代理讲解：

在上面的示例中，Email 是抽象主题，定义了发送邮件的方法 send()。

EmailServer 是真实主题，实现了 send() 方法来发送邮件。

EmailProxy 是代理主题，它实现了 send() 方法，并且内部持有一个 EmailServer 对象，以便在需要时代理访问 EmailServer 对象。

在 send() 方法中，它首先检查是否允许发送邮件，然后调用 EmailServer对象的 send() 方法来发送邮件，并在发送完成后记录日志。

最后，我们在客户端中创建了一个 EmailServer 对象和一个EmailProxy 对象，然后通过 EmailProxy 对象来发送邮件。

需要注意的是，在代理模式中，代理主题和真实主题必须实现同样的接口，因此代理主题必须是抽象主题的子类。此外，代理主题还可以通过实现额外的方法来增加一些附加的功能


## 行为型模式

行为型模式关注**对象之间的交互、通信和控制流**，大多数行为型模式基于组合和委托，而不是继承


### 1、职任链模式（Chain of Responsibility）

职责链模式（Chain of Responsibility）是一种行为型设计模式，它通过将请求的**发送者和接收者解耦**，从而使多个对象都有机会处理这个请求。

实现思路：

在职责链模式中，我们定义一系列的处理器对象，每个处理器对象都包含一个对下一个处理器对象的引用。

当请求从客户端发送到处理器对象时，**第一个处理器对象会尝试处理请求**，如果它**不能处理请求，则将请求传递给下一个处理器对象**，以此类推，直到请求被处理或者所有的处理器对象都不能处理请求。

优缺点：

职责链模式的优点是它可以灵活地配置处理器对象的顺序和组合，从而满足不同的处理需求。它还可以将请求的发送者和接收者解耦，从而提高系统的灵活性和可扩展性。

职责链模式的缺点是如果处理器对象过多或者处理器对象之间的关系过于复杂，可能会导致系统的维护难度增加。

职责链模式通常涉及以下角色：

处理器接口（Handler Interface）：定义处理器对象的接口，包含处理请求的方法和对下一个处理器对象的引用。

具体处理器类（Concrete Handlers）：实现处理器接口，处理请求或将请求传递给下一个处理器对象。

客户端（Client）：创建处理器对象的链，将请求发送给链的第一个处理器对象。

```
# 1、定义处理器接口：

class Handler:
    def set_next(self, handler):
        pass

    def handle(self, request):
        pass
```

```
# 2、实现具体处理器类：

class AbstractHandler(Handler):
    def __init__(self):
        self._next_handler = None

    def set_next(self, handler):
        self._next_handler = handler
        return handler

    def handle(self, request):
        if self._next_handler:
            return self._next_handler.handle(request)
        return None

class ConcreteHandler1(AbstractHandler):
    def handle(self, request):
        if request == "request1":
            return "Handled by ConcreteHandler1"
        else:
            return super().handle(request)

class ConcreteHandler2(AbstractHandler):
    def handle(self, request):
        if request == "request2":
            return "Handled by ConcreteHandler2"
        else:
            return super().handle(request)

class ConcreteHandler3(AbstractHandler):
    def handle(self, request):
        if request == "request3":
            return "Handled by ConcreteHandler3"
        else:
            return super().handle(request)
```

```
# 3、客户端创建处理器对象的链：

handler1 = ConcreteHandler1()
handler2 = ConcreteHandler2()
handler3 = ConcreteHandler3()

handler1.set_next(handler2).set_next(handler3)

# 发送请求
requests = ["request1", "request2", "request3", "request4"]
for request in requests:
    response = handler1.handle(request)
    if response:
        print(response)
    else:
        print(f"{request} was not handled")
```

代码讲解：

上面的示例中，我们定义了一个处理器接口 Handler，其中包含 set_next 和 handle 方法。

我们还定义了一个抽象处理器类 AbstractHandler，它实现了 set_next 和 handle 方法，其中 handle 方法调用了下一个处理器对象的 handle 方法。

我们还实现了三个具体的处理器类 ConcreteHandler1、ConcreteHandler2 和 ConcreteHandler3，它们分别实现了自己的 handle 方法。

客户端创建处理器对象的链，将处理器对象按照需要连接起来，然后将请求发送给链的第一个处理器对象，处理器对象将请求进行处理或者将请求传递给下一个处理器对象，直到请求被处理或者没有处理器对象能够处理请求。

在这个例子中，当请求为 "request1"、"request2"、"request3" 时，请求会被相应的处理器对象处理；当请求为 "request4" 时，没有处理器对象能够处理该请求，因此该请求未被处理。

总的来说，职责链模式可以使多个对象都有机会处理请求，并且可以灵活地配置处理器对象的顺序和组合，从而提高系统的灵活性和可扩展性。


### 2、命令模式（Command）

命令模式（Command）是一种行为型设计模式，它将**请求封装成一个对象**，从而使您可以**将不同的请求与其请求的接收者分开**。这种模式的目的是**通过将请求发送者和请求接收者解耦**来实现请求的发送、执行和撤销等操作。

实现思路：

在命令模式中，我们定义一个 Command 接口，该接口包含一个 execute 方法，用于执行命令。

我们还定义了一个 Invoker 类，它用于发送命令，可以接受一个 Command 对象，并在需要时调用该对象的 execute 方法。

我们还定义了一个 Receiver 类，它实际执行命令，包含一些特定于应用程序的业务逻辑。

命令模式涉及以下角色：

**Command 接口**：定义了一个执行命令的方法 execute。

**具体命令类（Concrete Command）**：实现了 Command 接口，实现 execute 方法，包含一个接收者对象，执行具体的业务逻辑。

**Invoker 类**：负责发送命令，它包含一个 Command 对象，可以在需要时调用该对象的 execute 方法。

**Receiver 类**：包含一些特定于应用程序的业务逻辑，实际执行命令。

```
# 1、定义 Command 接口：

from abc import ABC, abstractmethod

class Command(ABC):
    @abstractmethod
    def execute(self):
        pass
```

```
# 2、实现具体命令类：

class LightOnCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_on()

class LightOffCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_off()

```

```
# 3、定义 Invoker 类：

class RemoteControl:
    def __init__(self):
        self.commands = []

    def add_command(self, command):
        self.commands.append(command)

    def execute_commands(self):
        for command in self.commands:
            command.execute()
```

```
# 4、定义 Receiver 类：

class Light:
    def turn_on(self):
        print("The light is on")

    def turn_off(self):
        print("The light is off")
```

```
# 5、创建并使用命令：

light = Light()

remote_control = RemoteControl()
remote_control.add_command(LightOnCommand(light))
remote_control.add_command(LightOffCommand(light))

remote_control.execute_commands()
```

代码解释：

在这个例子中，我们首先定义了一个 Command 接口，该接口包含 execute 方法。然后，我们定义了两个具体命令类 LightOnCommand 和 LightOffCommand，它们实现了 Command 接口，并包含一个接收者对象 Light，实现了执行具体的业务逻辑。

我们还定义了一个 Invoker 类 RemoteControl，它包含一个 Command 对象的列表，并提供了一个 add_command 方法用于添加 Command 对象。execute_commands 方法用于在需要时调用 Command 对象的 execute 方法。

最后，我们定义了一个 Receiver 类 Light，它包含一些特定于应用程序的业务逻辑，实际执行命令。

在客户端代码中，我们创建了一个 Light 对象和一个 RemoteControl 对象。我们将 LightOnCommand 和 LightOffCommand 对象添加到 RemoteControl 对象的命令列表中，然后调用 execute_commands 方法来执行这些命令。

当我们执行这个程序时，它将输出以下内容：

The light is on

The light is off

这是因为我们创建了一个 Light 对象，然后使用 LightOnCommand 和 LightOffCommand 对象分别打开和关闭该对象。通过将**命令对象和命令的接收者对象分开**，我们可以**轻松地添加、删除和替换命令**，同时也使得程序更加灵活和可扩展。

总的来说，命令模式提供了一种通过将请求封装成对象来实现请求的发送、执行和撤销的方法，从而使得**命令对象和命令接收者对象解耦**，提高程序的灵活性和可扩展性。








