Socekt.io - Drawing App Workshop
--------------------------------

### Goals:
- Allow mulitiple users to share data with each other in real time
- Understand data flow  
  - Client establishes socket connection with server  
  - Client sends message to server - `.emit`  
  - Server receives message from client - `.on`  
  - Server sends client's message to **ALL** clients - `.emit()`  
  - **ALL** clients receive message from server - `.on()`
- Deploy application to Glitch using git and Github  

### PART 1 - SETUP
1. Download this entire repository
2. Open a Terminal Window and `cd` into the folder with the code files
3. Run `npm install` to load dependencies listed in `package.json`
4. Review starter code

### PART 2 - MAKE A SOCKET CONNECTION
4. SERVER - Initialize the HTTP server
```
//Initialize HTTP server
let http = require("http");
let server = http.createServer(app);
```
5. Update the `.listen()` line of code to user `server` instead of `app`
```
server.listen()
```
6. SERVER - Install socket.io via command line: `npm install socket.io` 
7. SERVER - Load and initialize socket.io in `index.js`
```
//Initialize socket.io
let io = require("socket.io");
io = new io.Server(server);
```
8. SERVER - Listen for individual clients to connect
```
//Listen for a client to connect and disconnect
io.on("connection", (socket) => {
  console.log("We have a new client: " + socket.id);
  
  //Listen for messages from the client


  //Listen for this client to disconnect
  socket.on("disconnect", () => {
    console.log("A client has disconnected: " + socket.id);
  });
});
```
9. CLIENT - Load socket library in `index.html` using a `<script>` tag
```
<script type="text/javascript" src="/socket.io/socket.io.js"></script>
```
10. CLIENT - Initialize socket and send resquest to the server to make socket connection
```
//Initialize and connect socket
let socket = io();
```
11. CLIENT - Listen for confirmation of socket connection
```
//Listen for confirmation of connection
socket.on('connect', () => {
  console.log("Connected");
});
```

### PART 3 - SEND A MESSAGE TO THE SERVER
12. CLIENT - Use p5 to grab the mouse position
```
function mouseMoved() {
  //Grab mouse position
  let mouseData = { x: mouseX, y: mouseY };

  //Draw yourself? or Wait for server?
  fill(0);
  ellipse(mouseX, mouseY 10, 10);
}
```
13. CLIENT - Send mouse the mouse data object to the server. Use `.emit()`
```
//Send mouse data object to the server
socket.emit('message', mouseData);
```
Also, comment out the following lines of code. Let's wait to draw the ellipse locally.
```
//fill(0);
//ellipse(mouseX, mouseY 10, 10);
```
14. SERVER - Listen for an event named `message` from client. Use `.on()`.
```
//Listen for an event named 'message' from client
socket.on('message', (data) => {
  console.log("Received 'message' with the following data:");
  console.log(data);

});
```

### PART 4 - SHARE THE MESSAGE WITH CLIENTS
15. SERVER - Share the message data with everyone. Use `.emit()`
```
//Send data to ALL clients, including this one
io.emit('message-share', data);

//Send data to ALL other clients but the sender
// socket.broadcast.emit('message-share', data);

//Send the data just to the sender
// socket.emit('message-share', data);
```
16. CLIENT - Receive the shared message. Use `.on()`.
```
//Listen for an event named 'message-share' from the server
socket.on('message-share', (data) => {
  console.log(data);

});
```
17. CLIENT - Draw an ellipse to the canvas.
```
drawEllipse(data);
```
```
//Expects an object with x and y properties
function drawEllipse(obj) {
  fill(0);
  ellipse(obj.x, obj.y, 10, 10);
}
```

### PART 5 - UPDATE CLIENT DATA
18. CLIENT - In `setup()`, assign a random fill color and ellipse size to the client
```
//Generate random fill values
let myRed = random(0,255);
let myGreen = random(0,255)
let myBlue = random(0,255);

//Generate random ellipse size
let myDiameter = random(5,25);
```
19. CLIENT - Update the mouse data being sent to the server
```
let mouseData = {
  x: mouseX,
  y: mouseY,
  r: myRed,
  g: myGreen,
  b: myBlue,
  d: myDiameter
}
```
20. CLIENT - Update the `drawEllipse()` function
```
fill(obj.r, obj.g, obj.b);
ellipse(obj.x, obj.y, obj.d, obj.d)
```

### PART 6 - PREPARE APP FOR GLITCH
21. Check `package.json` for `start` command
```
"start": "node index.js" //inside "scripts"
```
22. Check `index.js` for `port` variable
```
//Set the env variable for the port:
let port = process.env.PORT || 3000;
```
### PART 7 - PUSH TO GITHUB USING GIT
23. Create a local git repository
- `git init`
- `git status -s`
- `git add -A`
- `git status -s`
- `git commit -m"Initial commit"`

24. Create a remote git repository on Github
- Default is `Public`
- No need to generate a `README.md`, `.gitignore` or a license since we already have them. 
- Once created, we will use the **"or push an existing repository from the command line"** option. See Step #25.

25. Connect local git repo to remote Github repo
- `git remote add origin <url>`
- `git branch -M main`
- `git push -u origin main`

### PART 8 - DEPLOY TO GLITCH
26. On Gtihub, copy https `.git` url from Github
27. On Glitch, create a new project using "Import from Github". Paste in `.git` url from Github

