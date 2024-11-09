# Custom Events in Socket.io

Custom events in **Socket.io** are a powerful way to create real-time, bidirectional communication between the server and client. These events allow you to send and receive data through WebSockets based on your application's specific needs. By using custom events, you can implement unique functionality that is tailored to your app, such as user interactions, chat messages, game moves, notifications, etc.

#### 1. **What are Custom Events?**
Custom events are user-defined events that can be emitted and listened for on both the client and server. These events are not built-in events like `connect`, `disconnect`, etc., but are instead created specifically to match the application's requirements.

#### 2. **Emitting Custom Events**
Both the server and client can emit custom events using the `emit` method. The event can carry data as its payload, which can then be processed by the other side.

##### a. **Client-Side: Emitting Custom Events**
The client can emit custom events to the server by calling `socket.emit()`.

```javascript
// Client-side example: Emitting a custom event 'sendMessage'
const socket = io('http://localhost:3000');

// Emitting a custom event 'sendMessage' with a message payload
socket.emit('sendMessage', {
  message: 'Hello, Server!',
  sender: 'Client1'
});
```

In this example:
- `sendMessage` is the custom event emitted from the client.
- The second argument is the payload, which could be any type of data (e.g., an object, string, number).

##### b. **Server-Side: Emitting Custom Events**
The server can emit custom events to the client using `socket.emit()`.

```javascript
// Server-side example: Emitting a custom event 'messageReceived'
const io = require('socket.io')(3000);

io.on('connection', (socket) => {
  // Emitting a custom event 'messageReceived' with a response payload
  socket.emit('messageReceived', {
    status: 'success',
    message: 'Message has been successfully received.'
  });
});
```

Here:
- The server emits the `messageReceived` event back to the client with a status message.

#### 3. **Listening for Custom Events**
The client and server both listen for custom events using the `on()` method. The `on()` method takes the event name and a callback function that is invoked whenever that event is received.

##### a. **Client-Side: Listening for Custom Events**

```javascript
// Client-side example: Listening for 'messageReceived' event from the server
socket.on('messageReceived', (data) => {
  console.log('Server response:', data.message);  // Logs 'Message has been successfully received.'
});
```

Here:
- The client listens for the `messageReceived` event.
- When the event is received, the client processes the data and performs any necessary action, such as updating the UI.

##### b. **Server-Side: Listening for Custom Events**

```javascript
// Server-side example: Listening for 'sendMessage' event from the client
io.on('connection', (socket) => {
  socket.on('sendMessage', (data) => {
    console.log('Received message:', data.message);  // Logs 'Hello, Server!'
    // You can process the received message and respond back
  });
});
```

In this case:
- The server listens for the `sendMessage` event.
- When it’s received, the server processes the data and responds appropriately.

#### 4. **Handling Multiple Arguments in Custom Events**
Both the client and server can send multiple arguments with a custom event. These can be handled by the callback function that listens for the event.

##### Example: Sending Multiple Arguments
```javascript
// Client-side: Sending multiple arguments
socket.emit('sendData', 'Hello', 42, { user: 'Client1' });

// Server-side: Receiving multiple arguments
socket.on('sendData', (greeting, number, userData) => {
  console.log(greeting);  // 'Hello'
  console.log(number);    // 42
  console.log(userData);  // { user: 'Client1' }
});
```

Here:
- The client sends three different arguments in the `sendData` event.
- The server receives these arguments in the specified order and can process them as needed.

#### 5. **Acknowledgments with Custom Events**
Socket.io allows you to set up **acknowledgments** for custom events. This means the client can send an acknowledgment (callback function) that the server can invoke once it has processed the event.

##### a. **Client-Side: Sending with Acknowledgment**
```javascript
// Client-side: Emitting a custom event with an acknowledgment callback
socket.emit('sendMessage', { message: 'Hello, Server!' }, (response) => {
  console.log('Acknowledgment from server:', response);  // Server's response
});
```

##### b. **Server-Side: Sending with Acknowledgment**
```javascript
// Server-side: Emitting a custom event with an acknowledgment function
io.on('connection', (socket) => {
  socket.on('sendMessage', (data, ack) => {
    console.log('Received message:', data.message);  // 'Hello, Server!'
    // Send acknowledgment to the client
    ack('Message received and processed');
  });
});
```

In this example:
- The client sends the `sendMessage` event with an acknowledgment callback.
- The server processes the event and calls the acknowledgment callback (`ack`) with a response.

This ensures the client knows when the server has successfully processed the event.

#### 6. **Broadcasting Custom Events**
You can also **broadcast** custom events to all clients (or a subset of clients) connected to the server. Broadcasting is useful for sending real-time updates to multiple clients, like in a chat room or multiplayer game.

##### a. **Server-Side: Broadcasting to All Clients**
```javascript
// Broadcasting a custom event 'newMessage' to all clients
io.emit('newMessage', { message: 'A new message is available!' });
```

##### b. **Server-Side: Broadcasting to a Specific Room**
You can use **rooms** in Socket.io to broadcast custom events to a specific subset of clients.

```javascript
// Joining a room
socket.join('room1');

// Broadcasting to a specific room
io.to('room1').emit('newMessage', { message: 'Hello, Room 1!' });
```

In this case:
- Clients can join rooms, and the server can broadcast events to all clients in that room.

#### 7. **Custom Event Error Handling**
Custom events can encounter errors, so it’s important to handle them gracefully. The server can emit error events, and the client can listen for these events.

##### a. **Server-Side: Emitting Errors**
```javascript
// Emitting an error event to the client
socket.emit('error', 'Something went wrong!');
```

##### b. **Client-Side: Listening for Errors**
```javascript
// Listening for the error event on the client-side
socket.on('error', (errorMessage) => {
  console.error('Error from server:', errorMessage);  // 'Something went wrong!'
});
```

This provides a simple way to communicate errors back to the client.

#### 8. **Summary of Custom Events in Socket.io**

- **Emit**: Both the server and client can emit custom events using `socket.emit(eventName, data)`.
- **Listen**: Both the server and client listen for custom events using `socket.on(eventName, callback)`.
- **Multiple Arguments**: Custom events can carry multiple arguments, and the callback functions will receive them in order.
- **Acknowledgments**: You can use callback functions to send acknowledgment responses between the server and client.
- **Broadcasting**: Use `io.emit()` to send an event to all connected clients or use rooms to target a subset of clients.
- **Error Handling**: Emit custom error events and have the client listen for and handle them.

By using custom events, you can implement real-time, dynamic communication in your application, providing rich functionality for various use cases, including chat apps, real-time games, notifications, and more.