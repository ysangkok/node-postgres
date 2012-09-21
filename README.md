**SSL/TLS not supported** (use secure WebSockets instead)

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
    <script>
      var pg = require('pg');

      // conString must be object! URL's not supported (they are in the dns module, not available in the browser)
      var conString = {"host": "localhost", "port": 5432, "password": "test", "user": "janus", "database": "janus"};
      var client = new pg.Client(conString);
      // ...
    </script>
  </head>
  <body>
    <!-- Check the console when running! -->
  </body>
</html>
```

Building
--------
```
browserify -a dns:net -r generic-pool -r $PWD/pg/lib/index.js:pg $(for i in $PWD/pg/lib/*.js; do echo -n "-r $i "; done) -o bundle.js
```

Now open ```bundle.html``` over HTTP (make sure XHR permits WebSocket connections from that server to the WebSocket server!)
