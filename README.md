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











