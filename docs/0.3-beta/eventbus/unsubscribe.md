# Unsubscribe

Every message endpoint and event listener returns EventBusSubscription object 
that you can use to stop subscription

For message endpoint:
```csharp
EventBusSubscription subscription = this.MessageEndpoint<int, int>("address", message => {...});
...
subscription.Stop();
```
For event listener:
```csharp
EventBusSubscription subscription = this.EventListener("event-name", () => {});
...
subscription.Stop();
```