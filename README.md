# etcd-registry-router

Route http requests to services registered in etcd-registry

	npm install etcd-registry-router

## Usage

etcd-registry-router is a http server that forwards requests to services
registered in [etcd-registry](https://github.com/mafintosh/etcd-registry).

``` js
var router = require('etcd-registry-router');
var server = router('127.0.0.1:4001', function(request, route) {
	route('http/'+request.headers.host);
});

server.on('route', function(request, response, service) {
	console.log('Routing', request.url, 'to', service.name);
});

server.listen(8080);
```

The above snippet will start the router and route requests to services registered under `http/{host-header}`.
To create a service that accepts all requests routed to `example.com` do.

``` js
var registry = require('etcd-registry');
var services = registry('127.0.0.1:4001');

var server = http.createServer(function(request, response) {
	response.end('hello from service')
});

server.listen(0, function() { // listening on 0 will just give you a free port
	services.join('http/example.com', {port:server.address().port});
});
```

## License

MIT