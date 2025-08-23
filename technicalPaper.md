# Technical Report: Applying SOLID Principles for Codebase Refactoring

## Introduction

When joining a new project, developers often face challenges in maintaining and extending the codebase. Poor design leads to tightly coupled, hard-to-test, and unmanageable code. To solve this, the **SOLID principles** of object-oriented design provide guidelines for writing clean, extensible, and maintainable code.

This report introduces SOLID principles with simple **JavaScript** code examples to demonstrate how they improve project quality.

---

## 1. Single Responsibility Principle (SRP)

**Definition:** A class should have only one reason to change.

### Bad Example

```js
class Report {
  constructor(content) {
    this.content = content;
  }

  saveToFile(filename) {
    const fs = require('fs');
    fs.writeFileSync(filename, this.content);
  }
}
```

Here, `Report` handles both **data** and **persistence**.

### Good Example

```js
class Report {
  constructor(content) {
    this.content = content;
  }
}

class ReportSaver {
  saveToFile(report, filename) {
    const fs = require('fs');
    fs.writeFileSync(filename, report.content);
  }
}
```

Now, responsibilities are separated.

---

## 2. Open/Closed Principle (OCP)

**Definition:** Software entities should be open for extension but closed for modification.

### Bad Example

```js
class AreaCalculator {
  calculate(shape) {
    if (shape.type === 'circle') {
      return Math.PI * shape.radius ** 2;
    } else if (shape.type === 'rectangle') {
      return shape.width * shape.height;
    }
  }
}
```

New shapes require modifying the class.

### Good Example

```js
class Shape {
  area() {}
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }
  area() {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }
  area() {
    return this.width * this.height;
  }
}

class AreaCalculator {
  calculate(shape) {
    return shape.area();
  }
}
```

New shapes can be added without modifying `AreaCalculator`.

---

## 3. Liskov Substitution Principle (LSP)

**Definition:** Subtypes should be substitutable for their base types.

### Bad Example

```js
class Bird {
  fly() {}
}

class Ostrich extends Bird {
  fly() {
    throw new Error("Ostrich can't fly");
  }
}
```

This violates LSP because `Ostrich` cannot replace `Bird`.

### Good Example

```js
class Bird {}

class FlyingBird extends Bird {
  fly() {}
}

class Sparrow extends FlyingBird {
  fly() {
    console.log("Sparrow flying");
  }
}

class Ostrich extends Bird {
  run() {
    console.log("Ostrich running");
  }
}
```

Now, classes behave correctly according to their types.

---

## 4. Interface Segregation Principle (ISP)

**Definition:** Clients should not be forced to depend on methods they don’t use.

### Bad Example

```js
class Worker {
  work() {}
  eat() {}
}

class Robot extends Worker {
  eat() {
    throw new Error("Robot does not eat");
  }
}
```

`Robot` is forced to implement `eat` unnecessarily.

### Good Example

```js
class Workable {
  work() {}
}

class Eatable {
  eat() {}
}

class Human extends Workable {
  work() {
    console.log("Human working");
  }
  eat() {
    console.log("Human eating");
  }
}

class Robot extends Workable {
  work() {
    console.log("Robot working");
  }
}
```

Responsibilities are separated into smaller interfaces.

---

## 5. Dependency Inversion Principle (DIP)

**Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions.

### Bad Example

```js
class LightBulb {
  turnOn() {
    console.log("LightBulb ON");
  }
  turnOff() {
    console.log("LightBulb OFF");
  }
}

class Switch {
  constructor(bulb) {
    this.bulb = bulb;
  }
  operate() {
    this.bulb.turnOn();
  }
}
```

`Switch` depends directly on `LightBulb`.

### Good Example

```js
class Switchable {
  turnOn() {}
  turnOff() {}
}

class LightBulb extends Switchable {
  turnOn() {
    console.log("LightBulb ON");
  }
  turnOff() {
    console.log("LightBulb OFF");
  }
}

class Fan extends Switchable {
  turnOn() {
    console.log("Fan ON");
  }
  turnOff() {
    console.log("Fan OFF");
  }
}

class Switch {
  constructor(device) {
    this.device = device;
  }
  operate() {
    this.device.turnOn();
  }
}
```

Now `Switch` can work with any `Switchable` device.

---

## Conclusion

Applying the **SOLID principles** helps in refactoring a messy codebase into a clean, modular, and scalable structure. This makes the project easier to manage, extend, and test over time.

---

## References

* [DigitalOcean: SOLID Principles](https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)
* [SOLID Principles YouTube Playlist](https://www.youtube.com/playlist?list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme)
