#**Singleton**


----------
###*Sample Code:*
```
class Subject;

class Observer {
public:
  virtual ~Observer();
  virtual void Update(Subject* theChangedSubject) = 0;
protected:
  Observer();
};

class Subject {
public:
  virtual ~Subject();
  virtual void Attach(Observer*);
  virtual void Detach(Observer*);
  virtual Notify();
protected:
  Subject();
private:
  List<Observer*> *_observers;
};

void Subject::Attach (Observer* o) {
  _observers.Append(o);
}

void Subject::Detach (Observer* o) {
  _observers.Remove(o);
}

void Subject::Notify() {
  ListIterator<Observer*> i(_observers);

  for (i.First(); !i.IsDone(); i.Next()) {
    i.CurrentItem()->Update(this);
  }
}
```
We give the high level code of the Subject and Observer.
Any one who want to use this pattern can inherit the above class.

 The above sample code just map one observer to one subject, if you need to change the code also need to change. 

We can also use a `class ChangeManager` to deal with this staff.



###*Sample Code:*
```
class ChangeManager {
public:
  void Register(Subject*, Observer*);
  void UnRegister(Subject*, Observer*);
  void Notify();
private:
  // Subject-Observer Mapping
};

void ChangeManager::Notify() {
  forall s in subjects
    forall o in s.observers
      o->Update(s)
}
```

We can use `class ChangeManager` to manager all the subject and observer. the `Mapping` and the `Notify` can be implemented by the strategies you like.

###*Sample Code:*
```
void Subject::Attach(Observer*, Aspect& interest);
void Observer::Update(Subject*, Aspect& interest);
```

Here we add the `Aspect& interest`. In this way, we can only observe the thing we interested in. For example, in front-end development, we often use `onclick(...)`, `onkeydown(...)` for only one Dom object.


##**conclusion**
Benefit:

 1.  we can decouple the subject and the observer, so we can later reuse one of them without considering the other.
 2.  when we are coding, we can focus on the only one part of them, which satisfy separation of concerns.
 
Implement Point need to notice:
 
 1. Mapping subjects to their obervers.
 2. Oberveing more than one subject.
 3. Who triggers the update.
 4. Dangling references to deleted subjects.
 5. Making sure Subject state is self-consistent before notification.
 6. Avoiding observer-specific update protocols: push model and pull model.
 7. Specifying modifications of interest explicitly.
 8. Encapsulating complex update strategy. ( *like ChangeManager*