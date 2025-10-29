I built a virtual office workspace — kind of like a small metaverse — where users can join online rooms, move their avatars around a 2D map, talk through voice or video, and collaborate on a shared whiteboard. The frontend is developed using React with TypeScript and Vite for a fast and modular interface, while Phaser.js powers the 2D environment where users can freely move their avatars. On the backend, I used Node.js with Colyseus to handle real-time room management, synchronization, and player movements, along with MongoDB to store user and room data. For communication, WebRTC enables live audio and video streaming between users, and Socket.io powers the collaborative whiteboard, ensuring that all drawings and updates appear instantly for everyone.

For the frontend, I used React with TypeScript and Vite to build the main user interface — things like the login screen, room selection page, and the workspace screen. Inside the workspace, I used Phaser.js, which is a 2D game engine that runs directly in the browser and renders the virtual office map where users can move their avatars.

To design the map, I used a tool called Tiled, which lets you visually create maps using tiles — like floors, walls, and furniture. In Tiled, I also set collision properties for certain tiles (like walls or tables) so that players cannot walk through them. After exporting the map as a JSON file, I loaded it into Phaser.

In Phaser, the map becomes a “scene,” and each player’s avatar is represented as a “sprite,” which is basically a movable image. Phaser’s Arcade Physics system automatically handles movement and collisions. So, when a user presses the arrow keys or WASD, Phaser changes the avatar’s velocity and updates its position smoothly on the map without me manually calculating coordinates.

Now, Phaser only controls what happens visually on your own screen. To make it multiplayer, I connected Phaser with Colyseus, which handles the real-time data between all players. Whenever a player moves, their current coordinates are sent to the Colyseus server. The server updates the shared state (list of all players and their positions) and sends it back to everyone else.
Phaser then listens for those updates and moves the other players’ sprites accordingly — so you can see everyone walking around the same map in real time.

When a user moves their character, it first moves instantly on their own screen — this makes the game feel smooth and fast.
Then, that movement is sent to the server (Colyseus), which checks it and updates everyone else’s screen so all players see the same thing.

The server is the main source of truth — it makes sure no one moves too fast or cheats.
Every few milliseconds, the server tells all players the new positions of everyone in the room.
Each player’s screen then smoothly updates the movement using small animations so it doesn’t look laggy

In Phaser, every sprite has built-in x and y properties that update in real time as it moves.
You can read player.x and player.y anytime to know its exact position — and send those coordinates to your backend (Colyseus) so other players see your movement.


What is Colyseus.js and why I used it:
Colyseus.js is the client-side library for Colyseus — it helps the game (built in Phaser or React) connect to the multiplayer server using WebSockets. It automatically handles joining rooms, syncing game state (like players, positions, chat), and sending messages to the server in real time.
I used it because it makes multiplayer logic very easy — I don’t have to manually manage socket connections or data updates. It keeps all players in sync automatically, which is perfect for real-time apps like my virtual office project.

Main functions I used:

new Colyseus.Client("ws://localhost:3000") → connects the frontend to the Colyseus server.

client.joinOrCreate("room-name") → joins an existing room or creates one if it doesn’t exist.

room.send("messageType", data) → sends messages like movement or actions to the server.

room.state → holds the shared game state (like all players and their positions).

room.onMessage("messageType", callback) → listens for custom messages from the server (e.g., updates or notifications).


WebSockets are a technology that allows real-time, two-way communication between a client (like a browser) and a server over a single, long-lived connection. Unlike HTTP, where the client has to keep sending new requests to get updates, WebSockets keep the connection open so both sides can send messages anytime. The connection starts as a normal HTTP request — the browser asks the server to “upgrade” to a WebSocket by sending special headers like Upgrade: websocket. If the server agrees, it replies with a 101 Switching Protocols response, and from that moment, the link switches from HTTP to WebSocket. After that, the client and server can instantly exchange data in both directions without needing to reload or reconnect.


In my project, the whiteboard works in real time using Socket.io, which allows all users in the same room to see updates instantly. When a user starts drawing, their screen immediately shows the stroke locally (so it feels smooth), and at the same time, that drawing data — like color, thickness, and the x-y points — is sent to the server through Socket.io. The server then broadcasts that drawing event to everyone else in the same room, so all users’ whiteboards update together. When a new user joins, the server sends them the full whiteboard data so they can see everything that’s already been drawn. Actions like undo, erase, or clear are also shared as socket events, and the server keeps the board’s state consistent for everyone. This setup gives real-time collaboration with almost no delay and ensures everyone always sees the same content.


In simple words — every user that connects to a Socket.io server automatically gets a unique socket ID, which helps identify that specific connection. If we want to group users together (like in a shared room or whiteboard), we can create our own unique room ID using something like a UUID. The server then makes each user join that room using socket.join(roomId), and any events (like drawings or messages) sent to that room are broadcast only to the users inside it using io.to(roomId).emit(...). This way, each whiteboard or virtual room stays completely separate, and users only see updates from the room they are currently in.


WebRTC stands for Web Real-Time Communication. It allows two browsers or apps to directly share audio, video, or data without passing everything through a central server. It’s the main technology behind live video calls, screen sharing, and peer-to-peer chats.

The first step in WebRTC is capturing the local media. The browser asks for permission to access your microphone and camera using the getUserMedia() API. Once you allow, it gives a media stream that you can display on your own screen — for example, your local video tile in a call.

Next, both users need a way to find and connect to each other. WebRTC doesn’t handle that automatically, so we use a signaling server — usually through WebSockets, Socket.io, or Colyseus — to exchange connection details. During this stage, one user (the caller) creates an offer, and the other (the receiver) sends back an answer. Both these messages contain SDP, or Session Description Protocol, which describes what kind of media each side supports, like video codecs, resolution, and ports. Along with this, both users also exchange ICE candidates, which are network connection points that might work for establishing a link.

While all this signaling is happening, each peer sets up an RTCPeerConnection object. This is like the “bridge” that WebRTC uses to manage the real-time connection. The peer connection uses STUN servers to discover your public IP address — since most users are behind routers — and if a direct connection isn’t possible because of strict firewalls or NATs, it falls back to a TURN server, which acts as a relay to pass data between peers.

Once the offer, answer, and ICE candidates are exchanged through the signaling channel, both peers have enough information to connect directly. This is when the real peer-to-peer connection is established, and the media or data starts flowing between the two browsers — without going through your Node.js server anymore.

Finally, once the connection is stable, audio and video streams are transmitted directly between the peers in real time, providing very low latency. WebRTC can also open data channels, which are like mini sockets for sending messages, files, or game data instantly between users.

In your project, you probably used WebRTC in a peer-to-peer (P2P) mesh setup — which means every participant connects directly to every other participant in the room.

This works fine for 2 or 3 people, but when more users join, each browser has to send and receive multiple video/audio streams at once. For example, with 4 users in a mesh:

Each user must send 3 outgoing streams and receive 3 incoming streams — that’s 6 total connections per person.

This quickly increases CPU, memory, and bandwidth usage, especially when everyone’s streaming video. That’s why in your free or small setup, it might work unreliably or lag, because mesh networks don’t scale well.

In real production systems (like Zoom, Google Meet, or Discord), they use SFU (Selective Forwarding Unit) servers like mediasoup, Janus, or Jitsi.
An SFU acts like a “smart middleman” — each client sends one stream to the SFU, and the SFU forwards copies to all others. This drastically reduces the bandwidth load on clients and makes large meetings smooth.