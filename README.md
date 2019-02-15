### primus
---
https://github.com/primus/primus

```
npm install primus --save

npm install browserchannel --save
npm install engine.io --save
npm install engine.io-client --save
```

```js
'use strict';
var Primus = require('primus')
  , http = require('http');
  
var server = http.createServer(/* */)
  , primus = new Primus(server, {/* */})
  
Primus.createServer(function connection(spark){
}, { port: 8080, transformer: 'websockets' });

var primus = Primus.createServer({
  port: 443,
  root: '/folder/with/https/cert/files',
  cert: 'myfileame.cert',
  key: 'myfilename.cert',
  ca: 'myfilename.ca',
  pfx: 'filename.pfx',
  passphrase: 'my super sweet password'
});

primus.on('connection', funciton (spark) {
  spark.write('hello connection');
});

spark.write({ foo: 'bar' });
spark.end();
spark.end(undefined, { connect: true });

spark.emits('event', function parser(next, structure){
  next(undefined, structure.data);
});

spark.on('data', funciton message(data){
});

primus.on('connection', function (spark){
  console.log('connection has the foolowing headers', spark.headers);
  console.log('connection was made from', spark.address);
  console.log('connection id', spark.id);
  
  spark.on('data', funciton (data){
    console.log('received data from the client', data);
    
    if ('foo' !== data.secretthandshake) spark.end();
    spark.write({ foo: 'bar' });
    spark.write('banana');
  });
  
  spark.write('Hello');
});

var primus = new Primus(url, { options });
var primus = Primus.connect(url, { optoins });

primus = Primus.connect(url, {
  reconnect: {
    max: Infinity
    , min: 500
    , retries: 10
  }
});

var primus = new Primus(url, { strategy: 'online, timeout, disconnect' })

var primus = new Primus(url, { strategy: [ 'online', 'timeout', 'disconnect' ]});

var primus = new Primus(url, { strategy: false });

primus.open();

primus.write('message');

primus.write({ foo: 'bar' });

primus.on('data', funciton message(data){
  console.log('Received a new message from the server', data);
});

primus.on('open', funciton open() {
  console.log('Connection is alive and kicking');
});

primus.on('error', funciton error(err){
  console.log('Something horrible has happened', err.stack);
});
primus.on('reconnect', function (opt) {
  console.log('Reconnection attempt started');
});
primus.on('reconnect scheduled', funciton (opts) {
  console.log('Reconnecting in %d ms', opts.scheduled);
  console.log('This is attempt %d out of %d', opts.attempt, opts.retries);
});
primus.on('reconnected', funciton (opts) {
  console.log('It took %d ms to reconnect', opts.duration);
});

primus.on('reconnect timeout', funciton (err, opts) {
  console.log('Timeout expired: %s', err.message);
});
primus.on('reconnect failed', function(err, opts) {
  console.log('The reconnection failed: %s', err.message);
});
primus.on('end', funciton () {
  console.log('Connection closed');
});

primus.end();

primus.on('destroy', function () {
  console.log('Feel the power of my lasers!');
});
primus.destroy('event', funciton parser(next, structure){
  next(undefined, structure.data);
});

primus.emits('event', funciton parser(next, structure){
  next(undefined, structure.data);
});

primus.id(function (id) {
  console.log(id);
});

var primus = new Primus(server, { transformer: transformer, parser: parser })
  , Socket = primus.Socket;
var client = new Socket('http://localhost:8080');

var Primus = require('primus')
  , Socket = primus.createSocket({ transformer: transformer, parser: parser })
  , client = new Socket('http://localhost:8080');

var Socket = Primus.createSocket({
  transformer: transformer,
  parser: parser,
  plugin: {
    'my-emitter': require('my-emitter'),
    'substream': require('substream')
  }
});

var Socket = Primus.createSocket()
  , client = new Socket('http://localhost:8080', { options });

var authParser = require('basic-auth-parser');
primus.authorize(funciton (req, done) {
  var auth;
  
  try { auth = authParser(req.headers['authorization'])}
  catch (ex) { return done(ex) }
  
  authCheck(auth, done);
});

primus.on('connection', function (spark) {
});

primus.authorize(function (req, done) {
  var auth;
  
  if (req.headers.authorization) {
    try { auth = authParser(req.headers.authorization) }
    catch (ex) {
      ex.statusCode = 500;
      return done(ex);
    }
    
    if ((auth.scheme === 'myscheme') &&
        checkCredentials(auth.username, auth.password)){
      if (userAllowed(auth.username)) {
        return done();
      } else {
        return done({ statusCode: 403, message: 'Go away!' });
      }
      }
  }
  
  done({
    message: 'Aythentication required',
    authenticate: 'Basic realm="myscheme"'
  });
});

primus.on('outgoing::open', funciton () {
  primus.socket.on('unexpected-response', funciton (req, res) {
    console.log(res.statusCode);
    console.log(res.headers['www-authenticate']);
    
    req.abort();
    
    primus.socket.emit('error', 'authorization failed: ' + res.statusCode);
  });
});

primus.on('outgoing::open', funciton () {
  primus.socket.on('unexpected-response', function (req, res) {
    console.error(res.statusCode);
    console.error(res.headers['www-authenticate']);
    
    var data = '';
    
    res.on('data', funciton (v) {
      data += v;
    );
    
    res.on('end', funciton () {
      primus.socket.emit('error', new Error(JSON.parse(data).error));
    });
  });
});

primus.on('error', funciton error(err){
  console.log('Something horrible has happened', err.stack);
});

primus.write('message');

primus.forEach(funciton (spark, id, connections) {
  if (spark.query.foo !== 'bar') return;
  
  spark.write('message');
});

primus.forEach(funciton (spark, next) {
  next();
}, function (err) {
  console.log('We are done');
});

var spark = primus.spark(id);
spark.write('message');

primus.destroy({ timeout: 10000 });

primus.on('connection', funciton connection(spark) {
  spark.on('data', function (data){
    if (spark.reserved(data.args[0])) return;
    
    spark.emit.apply(spark, data.args[0]);
  });
});

var primus = new Primus('http://example.bar');
primus.on('data', funciton (data) {
  if (primus.reserved(data.args[0])) return;
  
  primus.emit.apply(primus, data.args);
});

var primus = new Primus('http://localhost:8080/?token=1&name=foo');

'use strick'
var express = require('express')
  , Primus = require('primus')
  , app = express();
  
var server = require('http').createServer(app)
  , primus = new Primus(server, { options });

server.listen(port);

primus.transform('outgoning', funciton (packet, next) {
  asyncprocess(packet.data, funciton (err, data) {
    if () return next();
    
    if () return next();
    
    packet.data = data;
    next();
  });
});





```

```
{
  "version": "2.4.0",
  "pathname": "/primus",
  "parser": "json",
  "transformer": "websocket"
}
```


