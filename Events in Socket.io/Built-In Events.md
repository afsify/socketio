# **Socket.io Built-In Events**

Socket.io provides a set of built-in events that are essential for real-time communication between the server and the client. These events are used for handling the lifecycle of socket connections, message broadcasting, disconnections, reconnections, and more.

Below is a comprehensive overview of the key built-in events in Socket.io:

---

### **1. `connection`**
- **Fires**: When a client successfully establishes a connection to the server.
- **Usage**: This event is emitted by the server whenever a client connects to it. You can listen for this event to handle new client connections.
  
  ```javascript
  io.on('connection', (socket) => {
    console.log('A user connected');
    // Additional logic for new connection
  });
  ```

---

### **2. `disconnect`**
- **Fires**: When a client disconnects from the server.
- **Usage**: This event is emitted when the connection is lost or the client disconnects explicitly. The reason for the disconnection is passed as a parameter.
  
  ```javascript
  io.on('connection', (socket) => {
    socket.on('disconnect', (reason) => {
      console.log(`User disconnected due to: ${reason}`);
    });
  });
  ```

- **Possible Reasons**:
  - `"ping timeout"`: If the server does not receive a ping within the timeout period.
  - `"transport close"`: If the connection is closed (e.g., network failure).
  - `"client namespace disconnect"`: When a client disconnects from a specific namespace.

---

### **3. `connect`**
- **Fires**: When the client successfully connects to the server.
- **Usage**: This event is emitted on the client side when a connection is successfully established with the server.

  ```javascript
  const socket = io();
  socket.on('connect', () => {
    console.log('Connected to server');
  });
  ```

---

### **4. `connect_error`**
- **Fires**: When there is an error while trying to connect to the server.
- **Usage**: This event is useful for handling connection issues such as network errors, invalid credentials, etc.

  ```javascript
  const socket = io();
  socket.on('connect_error', (error) => {
    console.error('Connection failed:', error);
  });
  ```

---

### **5. `reconnect`**
- **Fires**: When the client reconnects to the server after a disconnection.
- **Usage**: This event is triggered after the client successfully reconnects following a disconnection attempt. You can use this event to track successful reconnections.

  ```javascript
  const socket = io();
  socket.on('reconnect', () => {
    console.log('Reconnected to the server');
  });
  ```

---

### **6. `reconnect_attempt`**
- **Fires**: When the client attempts to reconnect after disconnection.
- **Usage**: This event is emitted each time the client tries to reconnect. It can be useful for tracking the number of reconnection attempts or adding custom logic (e.g., exponential backoff).

  ```javascript
  const socket = io();
  socket.on('reconnect_attempt', (attemptNumber) => {
    console.log(`Reconnection attempt #${attemptNumber}`);
  });
  ```

---

### **7. `reconnect_error`**
- **Fires**: When the client encounters an error while attempting to reconnect.
- **Usage**: This event is triggered when the client fails to reconnect after a certain number of attempts. It provides the error that caused the failure.

  ```javascript
  const socket = io();
  socket.on('reconnect_error', (error) => {
    console.error('Reconnection failed:', error);
  });
  ```

---

### **8. `reconnect_failed`**
- **Fires**: When the client has failed to reconnect after several attempts.
- **Usage**: This event is emitted when the client has exhausted all reconnection attempts. You can use this to display a message or perform other actions.

  ```javascript
  const socket = io();
  socket.on('reconnect_failed', () => {
    console.log('Reconnection failed. Please check your network or try again later.');
  });
  ```

---

### **9. `ping`**
- **Fires**: The server sends a `ping` event to the client to check if the connection is alive.
- **Usage**: This is used internally by Socket.io for heartbeats. The server can send a `ping` event to the client, and the client responds with `pong`. You generally do not need to handle this directly, but it can be customized for specific use cases.

---

### **10. `pong`**
- **Fires**: The client responds to the server's `ping` with a `pong` event.
- **Usage**: Similar to the `ping` event, it is used for heartbeats to ensure the connection is alive.

---

### **11. `error`**
- **Fires**: When there is an error during the communication between the client and the server.
- **Usage**: This event is used to send error messages or other errors that occur during the socket communication process. You can send custom error messages from the server to the client.

  ```javascript
  socket.emit('error', 'Custom error message');
  ```

---

### **12. `message`**
- **Fires**: This is a generic event for sending messages between the client and server.
- **Usage**: This event is often used for broadcasting messages to all connected clients or sending messages to specific clients. You can use `message` or create custom events for better clarity.

  ```javascript
  // Server-side
  io.emit('message', 'Hello, everyone!');

  // Client-side
  socket.on('message', (msg) => {
    console.log('Message from server:', msg);
  });
  ```

---

### **13. `namespace`**
- **Fires**: When a client connects to a specific namespace.
- **Usage**: Socket.io supports multiple namespaces, which are essentially different paths for socket connections. You can listen for a connection to a specific namespace.

  ```javascript
  // Server-side
  const nsp = io.of('/chat');
  nsp.on('connection', (socket) => {
    console.log('User connected to /chat namespace');
  });

  // Client-side
  const socket = io('/chat');
  ```

---

### **14. `broadcast`**
- **Fires**: This event is used to send a message to all connected clients except the sender.
- **Usage**: You can use `socket.broadcast.emit()` on the server to broadcast messages to all clients except the one that sent the message.

  ```javascript
  socket.broadcast.emit('message', 'New user has joined the chat');
  ```

---

### **15. `client message`**
- **Fires**: This is a custom event used for communication between the client and server, typically used for app-specific messages.
- **Usage**: You can use custom events to handle specialized functionality for your application, such as sending messages or data.

  ```javascript
  socket.on('client message', (data) => {
    console.log(data);
  });

  // Sending custom messages
  socket.emit('client message', { content: 'This is a message from the client' });
  ```

---

### **Summary of Key Built-In Events**
- **`connection`**: Triggered when a client connects to the server.
- **`disconnect`**: Triggered when a client disconnects.
- **`reconnect`**: Triggered after a successful reconnection.
- **`reconnect_attempt`**, **`reconnect_error`**, **`reconnect_failed`**: Events for handling the reconnection process.
- **`ping`** and **`pong`**: Used for heartbeats to ensure connection reliability.
- **`message`**: General message passing between client and server.
- **`broadcast`**: Send messages to all clients except the sender.

---

By understanding and leveraging these built-in events, you can better manage client-server interactions, connection status, and message exchanges within your real-time application.