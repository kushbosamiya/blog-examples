# Hey!!  Let's Make Chatroom using WebSocket

Creating a chatroom using WebSockets and React JS is a great way to add real-time functionality to your application. In this blog post, we will go over the steps to creating a basic chatroom using the WebSocket API and React JS.

First, let's start by setting up the WebSocket server. For this example, we will be using Node.js and the WS library. To install the WS library, you can use the following command:

```javascript
Copy codenpm install --save ws
```

Next, create a new file called server.js and add the following code:

```javascript
Copy codeconst WebSocket = require('ws');

const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
    ws.on('message', (message) => {
        console.log(`Received message => ${message}`);
        wss.clients.forEach((client) => {
            if (client.readyState === WebSocket.OPEN) {
                client.send(message);
            }
        });
    });
});
```

This code sets up a WebSocket server on port 8080 and handles incoming messages by broadcasting them to all connected clients.

Next, let's set up the React frontend for our chatroom. In your React project, install the following libraries:

```javascript
Copy codenpm install --save react-websocket
```

Then create a new component called ChatRoom.js and add the following code:

```javascript
Copy codeimport React, { useState } from 'react';
import WebSocket from 'react-websocket';

const ChatRoom = () => {
  const [messages, setMessages] = useState([]);
  const [text, setText] = useState('');

  const handleData = (data) => {
    setMessages((prevMessages) => [...prevMessages, data]);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    sendMessage();
  };

  const handleChange = (e) => {
    setText(e.target.value);
  };

  const sendMessage = () => {
    if (text.length > 0) {
      setMessages((prevMessages) => [...prevMessages, text]);
      setText('');
    }
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={text}
          onChange={handleChange}
        />
        <button type="submit">Send</button>
      </form>
      <WebSocket
        url={'ws://localhost:8080'}
        onMessage={handleData}
      />
      <div>
        {messages.map((message) => (
          <div key={message}>{message}</div>
        ))}
      </div>
    </div>
  );
};

export default ChatRoom;
```

This code sets up a simple form for sending messages and a WebSocket component that connects to our server on port 8080. The handle data function updates the message state with incoming messages, and the handle submits function sends the current text to the server.

Now, you can use this ChatRoom component in web application