# node-docker-monitor

This library is developed to perform one simple function: help to maintain list of running Docker containers on a single host.

## Install
Install locally
```
npm install node-grok
```

## Quick start
Following simple snippet starts monitoring containers on local host 

```javascript
var monitor = require('node-docker-monitor');

monitor({
    onContainerUp: function(container) {
        console.log('Container up: ', container)
    },

    onContainerDown: function(container) {
        console.log('Container down: ', container)
    }
});
```
Container object has following structure
```json
{
   "Command": "/bin/sh -c '/bin/bash -c 'cd /home; mkdir data; node main/app.js''",
   "Created": 1431402173,
   "Id": "81cde361ec7b069cc1ee32a4660176306a2b1d3a3eb52f96f17380f10e75d2e2",
   "Image": "m4all-next:15-0511-1104",
   "Names": ["/m4all-next"],
   "Ports": [
     {
       "IP": "172.17.42.1",
       "PrivatePort": 3000,
       "PublicPort": 3002,
       "Type": "tcp"
     }
   ],
   "Status": "Up About an hour",
   "Name": "m4all-next"
 }
```
When monitor starts, it calls `onContainerUp` callback for all currently running containers and then starts listening to Docker events, calling `onContainerUp` and `onContainerDown` when appropriate.

## API
* **function(handler, [docker])** - starts monitor with event *handler* that will receive events. Even handler must have `onContainerUp` and `onContainerDown` functions receiving parameters *info* - container info and *docker* - dockerode `Docker()` [object](https://github.com/apocas/dockerode). By default, it communicates to local Docker instance via `/var/run/docker.sock` Unix socket. If you want to change this behaviour, you can construct your own `Docker` object using [dockerode](https://github.com/apocas/dockerode) library and pass it as a second (optional) parameter.

## License 
**ISC License (ISC)**

Copyright (c) 2015, Andrey Chausenko <andrey.chausenko@gmail.com>

Permission to use, copy, modify, and/or distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.