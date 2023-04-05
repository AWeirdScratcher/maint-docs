# Maint API Documentation
This piece of documentation explains & demonstrates how you can connect your application with Maint.

## ⚡ Connecting to the Gateway
In order to connect to the Maint Gateway, you'll need to *prepare* these essentials:

- 🔑 A Maint Bot Token[^1]
- 🧦 A WebSocket Client[^2]

[^1]: A maint bot token is currently not available for the public. Stay tuned!
[^2]: We'll use Python and Javascript for demonstration purposes, as it's easy to understand and get started with.

Assuming we successfully connected to the Maint Gateway, it'd return the following data:

```json
{
  "t": 1
}
```

The `t` key reasonably represents the "type" of the data. In this case, the gateway returned `1`, which means it is asking us for the authorization token, or bot token.

When we received this type of data, we need to send a "pong" response to the server with our authorization token. The below JSON code example demonstrates what we should return to the server to fulfill the request.

```json
{
  "fulfill": "Bot <token>"
}
```

> **Warning** 
> <br />Maint no longer supports regular text requests (requests that are not returned in the JSON syntax).

If you still cannot understand the concept, the following diagram might help you out:

```py
client connected -> server asks for token -> client responds with the token -> ...
```

Since we've understood how the gateway works, the following code examples demonstrate how you can implement the "websocket connection".

**Javascript**
```js
// javascript

const ws = new WebSocket("wss://<maint gateway>")

ws.onconnect = (event) => {
  console.log("connected!")
}

ws.onmessage = (event) => {
  // "event.data" returns in plain text
  let data = JSON.parse(event.data); // convert to a valid JSON
  
  if (data.t == 1) {
    // the gateway is asking for our auth token
    ws.send(JSON.stringify({ // convert to plain text
      fulfill: "Bot <token>"
    }))
  }
}
```

**Python**

TODO: Write Python version

