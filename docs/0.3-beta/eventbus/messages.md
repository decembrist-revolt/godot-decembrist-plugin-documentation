# Message system
If you need a way to send \( _bidirectional_ \) messages from more than one sender to only one receiver,
you should use plugin's message system
<div align="center">
<img 
    src="_media/messages.png" 
    alt="Messages schema"/>
</div>

### Message endpoints
To register message endpoint use one of the following methods
##### Message endpoint with payload and reply
```csharp
class SomeNode : Node
{
    ...
        this.MessageEndpoint<int, int>("endpoint-address", (ReplyEventBusRequest<int, int> request) => {
            int requestPayload = request.Content;
            ...
            int response = 2;
            request.Reply(response);
        });
    ...
}
```
##### Message endpoint with payload and empty reply
```csharp
class SomeNode : Node
{
    ...
        this.MessageListener<int>("endpoint-address", (int? requestPayload) => {
            GD.PRINT(requestPayload);
        });
    ...
}
```
?> **this.MessageListener\<int>(...)** same as **this.MessageEndpoint<int, object?>(...)**
##### Message endpoint without payload and empty reply
```csharp
class SomeNode : Node
{
    ...
        this.NotificationListener("endpoint-address", () => {
            GD.PRINT("new endpoint-address message");
        });
    ...
}
```
?> **this.NotificationListener(...)** same as **this.MessageEndpoint<object?, object?>(...)**
### Send message
To send message use one of the following methods
##### Message with payload and reply
```csharp
class SomeNode : Node
{
    ...
        int requestPayload = 2;
        this.SendMessageRequest<int, int>("endpoint-address", requestPayload, (EventBusRequest<int> response) =>
        {
            int responsePayload = response.Content;
            string? errorMessage = response.Error;
            int? errorCode = response.ErrorCode;
        });
    ...
}
```
Async variant
```csharp
class SomeNode : Node
{
    ...
        int requestPayload = 2;
        try
        {
            int responsePayload = await this.SendMessageRequestAsync<int, int>("endpoint-address", requestPayload);
        }
        catch (SendEventException ex)
        {
            string? errorMessage = ex.Message;
            int? errorCode = ex.GetCode();
        }
    ...
}
```
##### Message with payload and empty reply
```csharp
class SomeNode : Node
{
    ...
        int requestPayload = 2;
        this.SendMessageAndForget<int>("endpoint-address", requestPayload);
    ...
}
```
Async variant
```csharp
class SomeNode : Node
{
    ...
        int requestPayload = 2;
        try
        {
            await this.SendMessageAndForgetAsync<int>("endpoint-address", requestPayload);
        }
        catch (SendEventException ex)
        {
            string? errorMessage = ex.Message;
            int? errorCode = ex.GetCode();
        }
    ...
}
```
?> **this.SendMessageAndForget<int>(...)** same as **this.SendMessageRequest<int, object?>(...)**
##### Message without payload and empty reply
```csharp
class SomeNode : Node
{
    ...
        this.SendNotification("endpoint-address");
    ...
}
```
Async variant
```csharp
class SomeNode : Node
{
    ...
        try
        {
            await this.SendNotificationAsync("endpoint-address");
        }
        catch (SendEventException ex)
        {
            string? errorMessage = ex.Message;
            int? errorCode = ex.GetCode();
        }
    ...
}
```
?> **this.SendNotification(...)** same as **this.SendMessageRequest<object?, object?>(...)**

!> For messages with payload, payload type for listener and emitter **should be the same**

### Request message methods ReplyEventBusRequest
```csharp
class SomeNode : Node
{
    ...
        this.MessageEndpoint<int, int>("endpoint-address", (ReplyEventBusRequest<int, int> request) => {
            int requestPayload = request.Content;
            ...
            int response = 2;
            // To response with payload
            request.Reply(response);
            // To response with empty payload
            request.EmptyReply();
            // To response error
            // User defined error message
            string errorMessage = "...";
            // User defined error code
            int errorCode = 100;
            request.ErrorReply(errorMessage, errorCode);
        });
    ...
}
```
### Unsubscribe
To unsubscribe use this guide [Unsubscribe](/0.3-beta/eventbus/unsubscribe.md)