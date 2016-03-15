# Why doesn't the bridge call to my JSNI method work in 

<some\_obj>

.onclick? #

While there are many reasons why your bridge call may not seem to be called in `onclick`, one of the most common causes has to do with function closures and how JavaScript variables are stored.

The code below illustrates this point:

```
public native void doSomething() /*-{
    this.@com.company.app.client.MyClass::doSomethingElse(Ljava/lang/String;)("immediate");
    someObj.onclick = function() {
        this.@com.company.app.client.MyClass::doSomethingElse(Ljava/lang/String;)("on click");
    }
}-*/;

public void doSomethingElse(String foo) {
    Window.alert(foo);
}
```

Someone new to JavaScript looking at this code may expect the "immediate" alert to display as soon as the code runs, and the "on click" alert to pop up when `someObj` is clicked. However, what actually will happen is that the first alert will display, but never the "on click." This problem occurs because "`this.@com...`" idioms create function closures over `this`. However, in JavaScript function closures, variables like `this` are stored by reference, not by value. Since the state of `this` isn't guaranteed once the JSNI `doSomething` function goes out of scope, by the time the `onclick` anonymous function callback gets run, this either points to some different object (which may not have a `doSomethingElse` method) or is even null or undefined. The fix is to create a local var that stores a copy of `this`, and creating the function closure over that. The snippet below shows the correct way to have the bridge call to `doSomethingElse` run when `someObj` is clicked.
```
public native void doSomething() /*-{
    var foo = this;
    this.@com.company.app.client.MyClass::doSomethingElse(Ljava/lang/String;)("immediate");
    someObj.onclick = function() {
        foo.@com.company.app.client.MyClass::doSomethingElse(Ljava/lang/String;)("on click");
    }
}-*/;
```

In this way, the variable over which we have created the closure is stored by value, and hence will retain that value even after the `doSomething` function goes out of scope.