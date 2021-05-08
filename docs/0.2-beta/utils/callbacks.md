# Callbacks

See **Godot** [Signals](https://docs.godotengine.org/en/stable/getting_started/scripting/c_sharp/c_sharp_features.html#c-signals)

### _Subscribe_
**Godot** _.Connect_ replacement  

_Instead of:_
```csharp
public void MyCallback()
{
    GD.Print("My callback!");
}

public void MyCallbackWithArguments(string foo, int bar)
{
    GD.Print("My callback with: ", foo, " and ", bar, "!");
}

public void SomeFunction()
{
    instance.Connect("MySignal", this, "MyCallback");
    instance.Connect(nameof(MySignalWithArguments), this, "MyCallbackWithArguments");
}
```
_With plugin you can do:_

```csharp
using Decembrist.Utils.Callback;

...

public void SomeFunction()
{
    //Lambda expressions
    instance.Subscribe("MySignal", () => {
        GD.Print("My callback!");
    });
    
    //Func delegate
    instance.Subscribe(nameof(MySignalWithArguments), MyCallbackWithArguments);
}
```
### _Unsubscribe_
To unsubscribe [Subscribed](/0.2-beta/utils/callbacks?id=subscribe-keyword) signal
```csharp
public void SomeFunction()
{
    var callback = instance.Subscribe("MySignal", () => {
        GD.Print("My callback!");
    });
    //Instead of Godot .Disconnect
    instance.Unsubscribe("MySignal", callback);
}
```