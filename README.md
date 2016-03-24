# Netflux WebSocket implementation

Implementation of the original Netflux API, based on a WebSocket server and a Star network topology.

## How to use

Here is an example describing how you can use the API in your code.

```javascript
require(['netflux.js'], function (Netflux) {

    var channel = 'ChannelName';
    var options = {
        signaling: url, // url of the WebSocket server
        topology: 'StarTopologyService',
        protocol: 'WebSocketProtocolService',
        connector: 'WebSocketService'
    };
    var onMessage = function(peer, message) {}; // Handler called when a message is received in the channel
    var onJoining = function(peer) {}; // Handler called when a user has joined the channel
    var onLeaving = function(peer) {}; // Handler called when a user has left the channel
    Netflux.join(channel, options).then(function(wc) {
        // Define the handlers
        wc.onMessage = onMessage;
        wc.onJoining = onJoining;
        wc.onLeaving = onLeaving;
    
        // Send a message when the user presses Enter in the input field
        var $entry = jQuery('#msg-entry');
        $entry.on('keydown', function (evt) {
            if (evt.keyCode !== 13) { return; }
            wc.send($entry.val()); // Broadcast the message in the channel
            $entry.val('');
        });
    
        // Request the history of this channel from the peer with the best link quality
        var hc;
        wc.peers.forEach(function (p) { if (!hc || p.linkQuality > hc.linkQuality) { hc = p; } });
        hc.send(JSON.stringify(['GET_HISTORY', wc.id]));
    
    }, function(error) {
        console.error(error);
    });

});
```
