Python 中的 `abc` 模块（Abstract Base Classes，抽象基类）提供了一种定义抽象基类的机制，用于强制子类实现特定的方法。

## 主要作用

- **定义接口规范**：规定子类必须实现的方法，类似于 Java 中的接口或抽象类
- **运行时类型检查**：通过 `isinstance()` 和 `issubclass()` 验证对象是否实现了特定接口
- **防止实例化**：抽象基类本身不能被实例化，只能被继承

## 核心组件

- `ABC`：抽象基类的基类，继承它可创建抽象基类
- `abstractmethod`：装饰器，标记必须在子类中实现的方法
- `abstractproperty`：抽象属性装饰器（Python 3.3+ 后可用 `@property` + `@abstractmethod` 组合）

## 基本用法示例

```python
from abc import ABC, abstractmethod

class Animal(ABC):  # 继承 ABC 成为抽象基类
    
    @abstractmethod
    def speak(self):
        pass  # 抽象方法，子类必须实现
    
    @abstractmethod
    def move(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof"
    
    def move(self):
        return "Running"

# dog = Animal()  # 错误！不能实例化抽象基类
dog = Dog()      # 正确
```

## 虚拟子类（Virtual Subclass）

使用 `register()` 方法可以将不相关的类注册为抽象基类的虚拟子类，无需实际继承：

```python
from abc import ABC

class MyABC(ABC):
    pass

MyABC.register(tuple)  # 将 tuple 注册为 MyABC 的虚拟子类
assert issubclass(tuple, MyABC)  # True
```

