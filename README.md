**SSL/TLS not supported** (use secure WebSockets instead)

```eventedapi.js``` prologue
----------------------------
```javascript
require('./pg/lib/generic-pool.js');
require('./pg/lib/writer.js');
require('./pg/lib/utils.js');
require('./pg/lib/binaryParsers.js');
require('./pg/lib/arrayParser.js');
require('./pg/lib/textParsers.js');
require('./pg/lib/types.js');
require('./pg/lib/result.js');
require('./pg/lib/query.js');
require('./pg/lib/client.js');
require('./pg/lib/defaults.js');
require('./pg/lib/connection.js');
var pg = require('./pg/lib/index.js');

// conString must be object! URL's not supported (they are in the dns module, not available in the browser)
var conString = {"host": "localhost", "port": 5432, "password": "test", "user": "janus", "database": "janus"};
var client = new pg.Client(conString);
// ...
```

Setting up postgres:

```bash
$ /usr/lib/postgresql/9.1/bin/initdb --locale=C .
$ $EDITOR postgresql.conf
$ /usr/lib/postgresql/9.1/bin/postgres -D . # in background tab
$ createdb -h $PWD -p 5432 # pwd = your former pwd of course
# stop background postgresql
$ ~/websockify/websockify 5432 -- /usr/lib/postgresql/9.1/bin/postgres -D .
```

```bundle.html``` contents
--------------------------

```html
<!doctype html>
<html>
  <head>
    <title>postgres</title>
    <meta charset="UTF-8" />
    <script src="/websockify/include/util.js"></script>
    <script src="/websockify/include/websock.js"></script>
    <script src="bundle.js"></script>
  </head>
  <body>
    <!-- Check the console when running! -->
  </body>
</html>
```

Building
--------
```
browserify -a 'dns:net' -e eventedapi.js -o bundle.js
```

Now open ```bundle.html``` over HTTP (make sure XHR permits WebSocket connections from that server to the WebSocket server!)