# scp2-deploy

A pure javascript secure copy program based on ssh2.

-----

scp2-deploy is greatly powered by [ssh2](https://github.com/mscdex/ssh2), implementing `scp` in an `sftp` way.

It is written in pure javascript, and should work on every OS, even Windows. Nodejs (v0.8.7 or newer) is required to use this library.


## Install

You can either use it as a library, or via CLI. For Windows users who miss scp in the unix/linux world, you can get it with:

    $ npm install scp2-deploy -g

You will get a command line tool `scp2-deploy`, now let's try:

    $ scp2-deploy -h


## High level API


Get the client:

```js
var client = require('scp2-deploy')
```

Copy a file to the server:

```js
client.scp('file.txt', 'admin:password@example.com:/home/admin/', function(err) {
})
```

You can also add the port to the url (default is 22):

```js
client.scp('file.txt', 'admin:password@example.com:port:/home/admin/', function(err) {
})
```

Copy a file to the server specifying the destination as an object:

```js
client.scp('file.txt', {
    host: 'example.com',
    username: 'admin',
    password: 'password',
    path: '/home/admin/'
}, function(err) {})
```

Copy a file to the server and rename it:

```js
client.scp('file.txt', 'admin:password@example.com:/home/admin/rename.txt', function(err) {
})
```

Copy a directory to the server:

```js
client.scp('data/', 'admin:password@example.com:/home/admin/data/', function(err) {
})
```

Copy via glob pattern:

```js
client.scp('data/*.js', 'admin:password@example.com:/home/admin/data/', function(err) {
})
```

Download a file from the server:

```js
client.scp('admin:password@example.com:/home/admin/file.txt', './', function(err) {
})
```

Download a file from the server specifying the destination as an object:

```js
client.scp({
    host: 'example.com',
    username: 'admin',
    password: 'password',
    path: '/home/admin/file.txt'
}, './', function(err) {})
```

Login with a private key:
```js
client.scp('file.txt', {
    host: 'example.com',
    username: 'admin',
    privateKey: require("fs").readFileSync('path/to/private/key'),
    passphrase: 'private_key_password',
    path: '/home/admin/'
}, function(err) {})
```

**TODO**: download via glob pattern.


## Low level API

Get the client:

```js
var Client = require('scp2-deploy').Client;
```

The high level client is an instance of `Client`, but also contains the high level API `scp`.

### Methods


- **defaults** `function({})`

  set the default values for the remote server.

  ```js
  client.defaults({
      port: 22,
      host: 'example.com',
      username: 'admin',
      privateKey: '....',
      // password: 'password', (accepts password also)
  });
  ```

  You can also initialize the instance with these values:

  ```js
  var client = new Client({
      port: 22
  });
  ```

  More on these values at [ssh2](https://github.com/mscdex/ssh2).


- **sftp** `function(callback) -> callback(err, sftp)`

  Get the sftp object.

- **close** `function()`

  Close all sessions.

- **mkdir** `function(dir, [attr], callback) -> callback(err)`

  Make a directory on the remote server. It behaves like `mdkir -p`.

- **write** `function(options, callback) -> callback(err)`

  Write content on the remote server.

  ```js
  client.write({
      destination: '/home/admin/data/file.txt',
      content: 'hello world'
  }, callback)
  ```

  The options object can contain:

  - destination
  - content: string or buffer
  - attrs
  - source: the source path, e.g. local/file.txt

- **upload** `function(src, dest, callback) -> callback(err)`

  upload a local file to the server.

  ```js
  client.upload('file.txt', '/home/admin/file.txt', callback)
  ```

- **download** `function(src, dest, callback) -> callback(err)`

  download a file from the server to local.


## Events

You can listen for these events:

- connect
- ready
- error (err)
- end
- close
- mkdir (dir)
- write (object)
- read (src)
- transfer (buffer, uploaded, total)


## License

Copyright (c) 2013 Hsiaoming Yang

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.
