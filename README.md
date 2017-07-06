# Template Method

Skeleton of an algorithm in an operation. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure

Base class declares algorithm 'placeholders', and derived classes
implement the placeholders


### When to use?
An application has significant similiarities on its components, but each type has its own small modification
  
## Participants
### Abstract Class
The AbstractClass, which defines the template method, and the signature of the sub parts of the algorithm are invoked by the template method.

### Concrete Class
The ConcreteClass implements abstract methods used by the template method of the AbstractClass. It is possible to have several concrete classes.
Collaboration

## Example
### Abstract class
```swift
class Report {

    let title: String
    let text:  [String]

    init(title: String, text: [String]) {
        self.title = title
        self.text = text
    }

    func outputReport() {
        outputStart()
        outputHead()
        outputBodyStart()
        outputBody()
        outputBodyEnd()
        outputEnd()
    }

    internal func outputStart() {
        preconditionFailure("this method needs to be overriden by concrete subscasses")
    }

    internal func outputHead() {
        preconditionFailure("this method needs to be overriden by concrete subscasses")
    }

    internal func outputBodyStart() {
        preconditionFailure("this method needs to be overriden by concrete subscasses")
    }

    private func outputBody() {
        text.forEach { (line) in
            outputLine(line)
        }
    }

    internal func outputLine(line: String) {
        preconditionFailure("this method needs to be overriden by concrete subscasses")
    }

    internal func outputBodyEnd() {
        preconditionFailure("this method needs to be overriden by concrete subscasses")
    }

    internal func outputEnd() {
        preconditionFailure("this method needs to be overriden by concrete subscasses")
    }
}
```

### Concrete Class
```swift
class HTMLReport: Report {

    override func outputStart() {
        print("<html>")
    }

    override func outputHead() {
        print("<head>")
        print("     <title>\(title)</title>")
        print("</head>")
    }

    override func outputBodyStart() {
        print("<body>")
    }

    override func outputLine(line: String) {
        print("     <p>\(line)</p>")
    }

    override func outputBodyEnd() {
        print("</body>")
    }

    override func outputEnd() {
        print("</html>")
    }
}
```
### Another Concrete class
```swift
class PlainTextReport: Report {

    override func outputStart() {}

    override func outputHead() {
        print("==========\(title)==========")
        print()
    }

    override func outputBodyStart() {}

    override func outputLine(line: String) {
        print("\(line)")
    }

    override func outputBodyEnd() {}

    override func outputEnd() {}
}
```

### Usage
```swift
let htmlReport = HTMLReport(title: "This is a a great report", text: ["reporting something important 1", "reporting something important 2", "reporting something important 3", "reporting something important 4"])

htmlReport.outputReport() 
```
### Output
```swift
<html>  
<head>  
     <title>This is a a great report</title>
</head>  
<body>  
     <p>reporting something important 1</p>
     <p>reporting something important 2</p>
     <p>reporting something important 3</p>
     <p>reporting something important 4</p>
</body>  
</html>  
```
### Usage
```swift
let plainTextReport = PlainTextReport(title: "This is a a great report", text: ["reporting something important 1", "reporting something important 2", "reporting something important 3", "reporting something important 4"])

plainTextReport.outputReport()  
```
### Output
```swift
==========This is a a great report==========

reporting something important 1  
reporting something important 2  
reporting something important 3  
reporting something important 4  
```

# Visitor

the visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to extant object structures without modifying the structures. It is one way to follow the open/closed principle.


### When to use?
An application with class hierarchy has to extend a new operation on each class.
  
## Example

### Without Visitor Pattern
```swift
protocol PlanetVisitor {
    func visit(planet: PlanetAlderaan)
    func visit(planet: PlanetCoruscant)
    func visit(planet: PlanetTatooine)
}

protocol Planet {
    func planetName(name: String)
}

class PlanetAlderaan: Planet {
	    func planetName(name: "Alderaan")
}
class PlanetCoruscant: Planet {
        func planetName(name: "Coruscant")
}
class PlanetTatooine: Planet {
	    func planetName(name: "Tatooine")
}
```
But proceeding in this way, each time you want to add an operation you must modify the interface to every single class of the hierarchy

### With Visitor Pattern

```swift
protocol PlanetVisitor {
    func visit(planet: PlanetAlderaan)
    func visit(planet: PlanetCoruscant)
    func visit(planet: PlanetTatooine)
}

protocol Planet {
    func accept(visitor: PlanetVisitor)
}

class PlanetAlderaan: Planet {
    func accept(visitor: PlanetVisitor) { visitor.visit(self) }
}
class PlanetCoruscant: Planet {
    func accept(visitor: PlanetVisitor) { visitor.visit(self) }
}
class PlanetTatooine: Planet {
    func accept(visitor: PlanetVisitor) { visitor.visit(self) }
}

class NameVisitor: PlanetVisitor {
    var name = ""

    func visit(planet: PlanetAlderaan)  { name = "Alderaan" }
    func visit(planet: PlanetCoruscant) { name = "Coruscant" }
    func visit(planet: PlanetTatooine)  { name = "Tatooine" }
}

```

Source : 

http://www.sm-cloud.com/template-method/

https://en.wikipedia.org/wiki/Template_method_pattern

https://en.wikipedia.org/wiki/Visitor_pattern

https://github.com/ochococo/Design-Patterns-In-Swift

Addison Wesley - Design Patterns Elements of Reusable Object Oriented Software
