# Netflux WebSocket implementation

Implementation of the Netflux API, based on a WebSocket server and a Star network topology.

## API specification (*warning*: early state)

### Remarks
Perhaps `WebChannel.openForJoining` and `WebChannel.closeForJoining` could be used in case of *WebRTC* and *WebSocket*. Maybe leave the possibility to the API user to decide either is an invite-only `WebChannel` or not. Lets implement and see.

### Facade

#### Attributes
- **onJoining**: function (someNew: *Peer*, oneOfMy: *WebChannel*)
  * When the *someNew* peer has joined the *oneOfMy* `WebChannel`.
- **onLeaving**: function (left: *Peer*, oneOfMy: *WebChannel*)
  * When the *left* peer has disconnected from the *oneOfMy* `WebChannel`.
- **onMessage**: function (from: Peer, to: WebChannel, data: string)
  * When a message has arrived from a `WebChannel`.
- **onPeerMessage**: function (from: *Peer*, data: *string*)
  * When some peer in the network has sent a message to you.
- **onInvite**: function (from: *Peer*,  data: *Object*)
  * When some peer in the network invites you to join one of his `WebChannel` and this using one of the `WebChannel` you are both connected to. (see `Peer.acceptInvite` and `Peer.rejectInvite`).

#### Methods
- **join** (key: *string*): *Promise*
- .then (function (net: *WebChannel*)): *Promise*
- .catch (function (err: *string*)): *Promise*
  * Join a `WebChannel` by providing the *key*. *key* is obtained from one of the peers who called `WebChannel.openForJoining`.
- **invite** (to: *Array [Peer]*, oneOfMy: *WebChannel*)
  * Send an invitation request to peer(s) to join *oneOfMy* `WebChannel` via another `WebChannel`(s).

___
### WebChannel

#### Methods
- **onJoining**: function (someNew: *Peer*, oneOfMy: *WebChannel*)
  * When the *someNew* peer has joined the *oneOfMy* `WebChannel`.
- **onLeaving**: function (left: *Peer*, oneOfMy: *WebChannel*)
  * When the *left* peer has disconnected from the *oneOfMy* `WebChannel`.
- **onMessage**: function (from: Peer, to: WebChannel, data: string)
  * When a message has arrived from a `WebChannel`.

#### Attributes
- **leave** (): *Promise*
- .then (function ()): *Promise*
- .catch (function (err: *string*)): *Promise*
  * Disconnect from this `WebChannel`.
- **send** (data: *string*): *Promise*
- .then (function ()): *Promise*
- .catch (function (err: *string*)): *Promise*
  * Send broadcast message to this `WebChannel`.
- **openForJoining** (): *string*
  * Enable people to join this `WebChannel`. This method return a key which must be provided by newcomers in order to join this `WebChannel`.
- **closeForJoining** ()
  * Disable people to join this `WebChannel` even if they have a key.

___
### Peer

- **send** (data: *string*): *Promise*
- .then (function ()): *Promise*  
- .catch (function (err: *string*)): *Promise*
  * Send a message to this peer via `WebChannel`.
- **Invite** (oneOfMy: Network)
  * Send an invitation request to peer(s) to join *oneOfMy* `WebChannel` via another `WebChannel`.
- **acceptInvite** ()
  * Accept invitation received from this peer which has been sent by him with `invite` method.
- **rejectInvite** ()
  * Reject join request received from this peer which has been sent by him with `invite` method.

## UML
Green and green/red parts (`Facade`, `WebChannel` and `Peer`) is what we consider to expose to the API user).

Gray parts represent some of internal elements of the API and they might change in the future (maybe find a new name for *Topology*).

![Netflux UML class diagram](doc/UML.png)
