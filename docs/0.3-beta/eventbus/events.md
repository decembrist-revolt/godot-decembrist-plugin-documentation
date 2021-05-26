# Event system
If you need a way to broadcast messages from one (or more) sender to one (or more) receivers,
you should use plugin's event system
<div align="center">
<img 
    src="_media/events.png" 
    alt="Messages schema"/>
</div>

### Event listeners
To register event listener use one of the following methods
##### Event with payload
```csharp
class SomeNode : Node
{
    ...
        this.EventListener<int>("event-name", (int eventPayload) => {
            GD.Print(eventPayload);
        });
    ...
}
```
##### Event without payload
```csharp
class SomeNode : Node
{
    ...
        this.EventListener("event-name", () => {})
    ...
}
```
?> **this.EventListener(...)** same as **this.EventListener<object?>(...)**
### Event emitters
To emit new event use one of the following methods
##### Event with payload
```csharp
class SomeNode : Node
{
    ...
        int eventPayload = 2;
        this.FireEvent("event-name", eventPayload);
    ...
}
```
##### Event without payload
```csharp
class SomeNode : Node
{
    ...
        this.FireEvent("event-name")
    ...
}
```
!> For events with payload, payload type for listener and emitter **should be the same** (or null)

### Unsubscribe
To unsubscribe use this guide [Unsubscribe](/0.3-beta/eventbus/unsubscribe.md)