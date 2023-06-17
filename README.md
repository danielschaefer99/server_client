# Game Server + Client Concept
This is an implementation of a game server and client structure using a [Deterministic Lockstep](https://www.gafferongames.com/post/deterministic_lockstep/) model with simulated TCP packet security over a UDP connection.
The program also utilizes an Entity-Component-System (ECS) as a proof of concept to support all of its functionality.

### Server ###
After initial setup of the listener and sender sockets, the server launches into a Lockstep loop with a tickrate of 60 ticks per second.
If a tick is completed quicker than in 1/60th of a second, the waiting time between ticks can be used to execute additional code as needed.

### Client ###
The client launches into a simple 2D top-down world with a moveable character to showcase the functioning concept. Upon launch, the client connects to the server and launches two sockets for listening and sending.

### Connection ###
When a Client tries to connect to the server, a connection request byte is sent. The server receives the request, adds the client to the active players and assigns it an ID. This ID is then sent through a specific byte-code back to the Client. Every tick, the server will then send the current positions of all players to the client. The client itself will send inputs to the server as soon as they're performed by the player, together with a timestamp. If the input-packet is not acknowledged by the server, the client will resend it until it is received. Additionally, the server uses the timestamp to sync up inputs as they're performed by different clients.

This implementation ensures that the outcome is Deterministic and the same between all clients.

The exact byte-codes used during server-client communications can be found in the "game_server_client_codes.txt" file inside the repository root. For the simple WASD-movement implemented here, the client simply sends 10 bytes per input performed and the server sends 14 bytes per tick, allowing for a very low bandwidth.

### Setup ###
To successfully launch the program after building it, the four DLL files inside the "./gameproject/DLLs" folder must be added to the execution directory. These DLLs simply add the [SFML.NET](https://www.sfml-dev.org/download/sfml.net/) framework, allowing for the display of 2D graphics.
