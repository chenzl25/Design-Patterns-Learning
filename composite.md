# Composite Pattern

## Intent
Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

## Participants
1. Component
  - declares the interface for objects in the composition.
  - implements default behavior for the interface common to all classes, as appropriate.
  - declares an interface for accessing and managing its child components.
  - (optional) defines an interface for accessing a component's parent in the recursive structure , and implements it if that 's appropriate.
2. Left
  - represents leaf objects in the composition. A leaf has no children.
  - defines behavior for primitive objects in the composition
3. Composite
  - defines behavior for components having children.
  - stores child components.
  - implements child-related operations in the Component interface
4. Client.
  - manipulates objects in the composition through the Component interface.

## Benefit:
1. Wherever client code expects a primitive object, it can also take a composite object.
2. makes the client simple, because it avoids having to write tag-and-case-statement-style functions over the classes that define the composition.
3. makes it easier to add new kinds of components. Clients don't have to be changed for new Component classes.

## Drawback
1. make your design overly general, because it is so easy to add new components so that it is hard to restrict the components of a composite. Sometimes you want a composite to have only certain components. In that time, type system can not help and we have to ask run-time checks for help.

## Safety & Transparency
One of the goals of the Composite pattern is to make clients unware of the specific Leaf or Composite. To attain this goal, the Component class should define as many common operations for Composite and Leaf classes as possible.
How about the `Add` and `Remove` method in the Composite? Should we keep it in the Component class?
1. Defining the child management interface at the root of the class hierarchy gives you transparency, because you can treat all components uniformly. It costs you safety, however because clients may try to do meaningless things like add and remove objects from leaves.
2. Defining child management in the Composite class give you safety, because any attempt to add or remove objects from leaves will be caught at compile-time in a statically typed language like C++. But you lose transparency, because leaves and composites have different interface.

## *Sample Code*
```
class Component {
  // virtual method
}

class Leaf: public Component {
  //...
}

class Composite: public Component {
 public:
  //...
  void Add(Component*);
  void Remove(Component*);
 private:
  List<Component*> _components;
}
```