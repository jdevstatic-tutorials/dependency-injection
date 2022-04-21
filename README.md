# Dependency Injection

*`updated 22 April 2022`*

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fxdvrx1%2Fjava-interface-lesson&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=PAGE+VIEWS&edge_flat=false)](https://hits.seeyoufarm.com)

At first, I was confused too, what is dependency injection
and why is it important in programming? 

Dependency Injection (DI) is very important as it will produce
loosely-coupled codebase that can be tested without much hassle.
It also achieves singleton pattern where you are sure that 
the object's state was not changed before you use that in
another class. Subtle changes are actually hard to be found. 
Lastly, the control is not on any class but on 
one class only that is responsible for creating objects. It's Inversion
of Control.

## An Example

`A.java`

```
//the object to be injected
class A {
    //a sample variable
    int number = 10;
}
```

`DependencyInjectionClass.java`

```
class DependencyInjectionClass {
    
    private A a;    
    
    DependencyInjectionClass(A a) {
        //`this.a` is pointing to the same
        //a object of A class
        //given this situation, we can now
        //do things on A's object safely
        this.a = a;   
    }
    
    public void businessLogic() {
        System.out.println("@DIClassBusinessLogicMethod");
        System.out.print("Before `a.number` is changed, `a.number` is: ");
        System.out.println(a.number);
        System.out.print("`a.number` is changed in this class, "
                               + "`a.number` is now: ");        
        a.number = 15;        
        System.out.println(a.number);
    }
    
}
```

`FactoryClass.java`

```
//this is the only place where we should create objects 
class FactoryClass {
    private A a = new A();  

    public DependencyInjectionClass dependencyInjectionClass() {
        return new DependencyInjectionClass(a);
    }
    
    public int returnANumber() {
        return a.number;   
    }
}
```

`Main.java`

```
class Main {
    public static void main(String args[]) {
        FactoryClass fc = new FactoryClass();

        DependencyInjectionClass 
            dependencyInjectionClass = fc.dependencyInjectionClass();
        dependencyInjectionClass.businessLogic();
        
        //there is only one instance of A
        //so we are always sure about the state of
        //the object, 
        //to test that, we call `a.number` from the 
        //factory class, and see whether number
        //variable was actually changed
        System.out.println("@FactoryClass called in Main");
        System.out.print("Last call to a.number: ");
        System.out.println(fc.returnANumber());
    }
}
```

result:

```
> run Main
@DIClassBusinessLogicMethod
Before `a.number` is changed, `a.number` is: 10
`a.number` is changed in this class, `a.number` is now: 15
@FactoryClass called in Main
Last call to a.number: 15
```

Take note, there are two A references, one in the `DependencyInjectionClass`,
the other one in `FactoryClass`. They are both declared as private. But, the 
object is only created in `FactoryClass`. For large-scale projects, you want
to be sure that the object's state is what actually is expected.
In testing, that is the test fixture. You want to make sure that
you test your code on at least one object's state. 

Frameworks like Spring does the job exactly.
