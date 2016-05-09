#**Singleton**


----------
###*Sample Code:*
```
class Singleton {
public:
  static Singleton* Instance();
  // another api go here
protected:
  Singleton();
private:
  static Singleton* _instance;
}

Singleton* Singleton::_instance = NULL;
Singleton* Singleton::Instance () {
  if (_instance == NULL) {
    _instance = new Singleton;
  }
  return _instance;
}
```
Benifit:

 1. Controlled access to sole instance. Instead of using static global varible, using a class can encapsulate the instance, which results in a better control.
 2. notice we can't not call the `new Singleton()` directly, because it is a protected method, which guarantee at most one instance will exist.
 2. Actually, we can change the number of the instance.
 3.  The Singleton class may be subclassed, we can configure the application with an instance of the class we need at run-time. 


----------
###*Sample Code:*
```
Singleton* Singleton::Instance () {
  if (_instance == NULL) {
    const char* singletonStyle= getenv("SingletonStyle");
    
    if (strcmp(singletonStyle, "SubClass1")) {
      _instance = new SubClass1();
    }
    if (strcmp(singletonStyle, "SubClass2")) {
      _instance = new SubClass2();
    }
    .....
    else {
      // default case
      _instance = new Singleton;
    }
  }
  return _instance;
}
```
In this case, we extend the origin `class Singleton`, we may have serval subclass. 
One point need to notice is that whenever we add a subclass we need to modify some code in the `class Singleton`. We can improve it to only modify the subclass itself, but notice that don't over designing. Things should be simple to understand.

----------
###*Sample Code:*
```
class Singleton {
public:
  static Singleton* Instance();
  static Register(const char* name, Singleton*);
  // another api go here
protected:
  static Singleton* Lookup(const char* name);
private:
  static Singleton* _instance;
  static List<NameSingletonPair>* _registry;
}

Singleton* Singleton::_instance = NULL;
Singleton* Singleton::Instance () {
  if (_instance == NULL) {
    const char* singletonStyle= getenv("SingletonStyle");
    _instance = Lookup(singletonStyle);
  }
  return _instance;
}

// here the subclass constructor
SubClass1::SubClass1() {
  Singleton::Register("SubClass1", this);
}
// using a static variable
static SubClass1 subClass1;
```
Some point to notice:

 1.  in this case, when we add a subclass, we can only modify a Single place and keep the `class Singleton` unmodified.
 2.  However,  the `class Singleton` ,now, is not responsible to create the singleton object. Instead, its primary responsibility is to make the singleton object of choice accessible in the system.
 3. we use the static variable approach to register the subclass, but we need to create all possible subclass.Otherwise, they won't get registered.
 
##**conclusion**
If we don't need to extend the Singleton the Sample Code 1 is well enough. Otherwise, I recommend the second Sample Code 2, because I think its is more easy to understand and can create the object when we need.


