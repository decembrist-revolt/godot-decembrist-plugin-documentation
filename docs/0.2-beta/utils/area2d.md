# Area2D

See **Godot** [Area2D](https://docs.godotengine.org/en/stable/tutorials/physics/using_area_2d.html)

### _OnPointerEnter_ 
Godot signal: [**mouse_entered**](https://docs.godotengine.org/en/stable/classes/class_collisionobject2d.html#class-collisionobject2d)

```csharp
area2D.OnPointerEnter(() => GD.Print("pointer enter"))
```

### _OnPointerExit_
Godot signal: [**mouse_exited**](https://docs.godotengine.org/en/stable/classes/class_collisionobject2d.html#class-collisionobject2d)
```csharp
area2D.OnPointerExit(() => GD.Print("pointer exit"))
```

### _OnInput_
Godot signal: [**input_event**](https://docs.godotengine.org/en/stable/classes/class_collisionobject2d.html#class-collisionobject2d)
```csharp
area2D.OnInput((Object viewport, InputEvent @event, int shapeIdx) => GD.Print("input"))
```