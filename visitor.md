#**Visitor**


----------

Visitor pattern is common in compiler design.
For a AST, we may do serval pass to do different thing, like: type-check, generate-code, optimization and other analysis.

**first we give some code in object-oriented style.**

----------
###*Sample Code:*
```
class Exp {
 public:
  virtual void eval();  
};

class PlusExp : public Exp {
 private:
  Exp *_left,*_right;
 public:
  PlusExp(Exp *left, Exp *right) {
    _left = left;
    _right = right;
  }
  virtual void eval() {
    _left->eval(); // push into stack
    _right->eval();// push into stack
    int right_value = Stack::getInstance()->pop();
    int left_value = Stack::getInstance()->pop();
    Stack::getInstance()->push(left_value + right_value);
  }
};

class MulExp : public Exp {
  // similar 
  virtual void eval();  
};
class SubExp : public Exp {
  // similar 
  virtual void eval();  
};
class DivExp : public Exp {
  // similar 
  virtual void eval();  
};
```

We can call in the root of AST with `root->eval()`.
This kind of modularity is good for us to modify the syntax and the structure of AST we are not certain now. 
However, if our AST's structure is certain and unchanged, It is better to use Visitor pattern, which is good for us to  add some action to the AST, without modifying the code of AST and recompilation.

**next we give some code in Visitor pattern**

###*Sample Code:*
```
class Exp {
 public:
  virtual void accept(Visitor* v);  
};

class PlusExp : public Exp {
 private:
  Exp *_left,*_right;
 public:
  PlusExp(Exp *left, Exp *right) {
    _left = left;
    _right = right;
  }
  virtual void accept(Visitor* v) {
    v->visit(this);
  }
};

class MulExp : public Exp {
  // similar 
  virtual void accept(Visitor* v) {
    v->visit(this);
  }
};
class SubExp : public Exp {
  // similar 
  virtual void accept(Visitor* v) {
    v->visit(this);
  } 
};
class DivExp : public Exp {
  // similar 
  virtual void accept(Visitor* v) {
    v->visit(this);
  } 
};

class Visitor {
 public:
  virtual void visitor(PlusExp* plus_exp);
  virtual void visitor(MulExp* mul_exp);
  virtual void visitor(SubExp* sub_exp);
  virtual void visitor(DivExp* div_exp);
};

class Interpreter: public Visitor {
 public:
  void visit(PlusExp* plus_exp) {
    plus_exp->_left->visit(this);
    plus_exp->_right->visit(this);
    int right_value = Stack::getInstance()->pop();
    int left_value = Stack::getInstance()->pop();
    Stack::getInstance()->push(left_value + right_value);
  }
  virtual void visitor(MulExp* mul_exp); 
  virtual void visitor(SubExp* sub_exp);
  virtual void visitor(DivExp* div_exp);
};
```

We can see that we decouple action from the structure of AST.
That the another kind of modularity, aks syntax separate from interpretation.

----------