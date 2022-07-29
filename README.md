
# Wosnin

A native zero depedancy websocket client and server for Deno. Built in routing and error handling.

## Usage/Examples

### Server Example

```typescript
import { opine, serveStatic, urlencoded, Router } from "https://deno.land/x/opine/mod.ts";

import { WosninServer } from "./Wosnin/server.ts";

const app = opine()
const ws = new WosninServer()
const port = 5000

app.get("/", (req,res) => {
  res.send("dipshit")
})

app.get("/ws", async (req, res) => {
  if (req.headers.get("upgrade") === "websocket") {
    const socket = req.upgrade()
    console.log("hit");
    ws.listen(socket)
  } else {
    res.send("Ok?")
  }
})

ws.route("/time", (req, sb) => {
  console.log(req.data);
  ws.send(sb, "cool, this works")
})

app.listen(port);
console.log(`Opine started on localhost:${port}`)
}
```

### Client Example

```typescript
import { WosninClient } from "./Wosnin/client.ts";

let ws = new WosninClient('ws://localhost:5000/ws')

ws.route("/help", (req: any)=> {
  ws.send(req)
})

//sends a message every 1 second
setInterval(()=>{ ws.send("{'route': "time"}") }, 1000 )
```
