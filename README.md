# mineflayer-navigate

A library to help your mineflayer bot navigate around the 3D world using
the A* algorithm.

See [https://github.com/superjoe30/mineflayer/](https://github.com/superjoe30/mineflayer/)

## Usage

```js
var mineflayer = require('mineflayer');
var navigatePlugin = require('mineflayer-navigate')(mineflayer);
var bot = mineflayer.createBot({ username: 'Player' });
// install the plugin
navigatePlugin(bot);
bot.on('chat', function(username, message) {
  // navigate to whoever talks
  if (username === bot.username) return;
  var target = bot.players[username].entity;
  if (message === 'come') {
    bot.navigate.to(target.position);
  } else if (message === 'stop') {
    bot.navigate.stop();
  }
});
```

## Documentation

### bot.navigate.to(point, options)

Finds a path to the specified location and goes there.

 * `point` - the block you want your feet to be standing on
 * `options` - optional parameters which come with sensible defaults
   - `isEnd` - function(node) - passed on to the A* library. `node.point` is
     a vec3 instance.
   - `endRadius` - used for default `isEnd`. Effectively defaults to 0.
   - `timeout` - passed on to the A* library. Default 10 seconds.

### bot.navigate.stop()

Aborts an in progress navigation job.

### event "pathPartFound" (path)

Emitted from `bot.navigate` when a partial path is found. `path` is an array
of nodes.

### event "pathFound" (path)

Emitted from `bot.navigate` when a complete path is found. `path` is an array
of nodes.

### event "cannotFind"

Emitted when a path cannot be found.

### event "arrived"

Emitted when the destination is reached.

### event "stop"

Emitted when navigation has been aborted.
