# Java 中的 Getters 和 Setters 解释

> 原文：<https://www.freecodecamp.org/news/java-getters-and-setters/>

Getters 和 setters 用于保护数据，尤其是在创建类的时候。

对于每个实例变量，getter 方法返回其值，而 setter 方法设置或更新其值。鉴于此，getters 和 setters 也分别被称为*访问器*和*变异器*。

按照惯例，getters 以单词“get”开头，setters 以单词“set”开头，后面跟一个变量名。在这两种情况下，变量名的首字母都是大写的:

```
public class Vehicle {
  private String color;

  // Getter
  public String getColor() {
    return color;
  }

  // Setter
  public void setColor(String c) {
    this.color = c;
  }
}
```

getter 方法返回属性的值。setter 方法接受一个参数，并将其赋给属性。

一旦定义了 getter 和 setter，我们就可以在我们的 main:

```
public static void main(String[] args) {
  Vehicle v1 = new Vehicle();
  v1.setColor("Red");
  System.out.println(v1.getColor());
}

// Outputs "Red"
```

Getters 和 setters 允许控制这些值。在实际设置值之前，您可以在 setter 中验证给定值。

### 为什么要使用 getters 和 setters？

Getters 和 setters 允许您控制在代码中访问和更新重要变量的方式。例如，考虑这个 setter 方法:

```
public void setNumber(int number) {
  if (number < 1 || number > 10) {
    throw new IllegalArgumentException();
  }
  this.number = num;
}
```

通过使用`setNumber`方法，您可以确保`number`的值总是在 1 和 10 之间。这比直接更新`number`变量要好得多:

```
obj.number = 13;
```

如果你直接更新`number`，你可能会在代码的其他地方引起意想不到的副作用。这里，将`number`设置为 13 违反了我们想要建立的 1 比 10 约束。

将`number`设为私有变量并使用`setNumber`方法可以防止这种情况发生。

另一方面，读取`number`值的唯一方法是使用 getter 方法:

```
public int getNumber() {
  return this.number;
}
```