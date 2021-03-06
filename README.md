# restify-await-promise

[![Build Status](https://travis-ci.org/PhinCo/restify-await-promise.svg)](https://travis-ci.org/PhinCo/restify-await-promise)
[![devDependency Status](https://david-dm.org/PhinCo/restify-await-promise/dev-status.svg)](https://david-dm.org/PhinCo/restify-await-promise#info=devDependencies)

Converts restify routes to support async/await and returned promises

# Usage

```javascript
const restify = require('restify');
const restifyPromise = require('restify-await-promise');

const server = restify.createServer({
  name: 'myapp',
  version: '1.0.0'
});

//Allows you to manipulate the errors before restify does its work
const alwaysBlameTheUserErrorTransformer = {
	transform: function( exceptionThrownByRoute ){
		//Always blame the user
		exceptionThrownByRoute.statusCode = 400; 
		return exceptionThrownByRoute;
	}
}

const options = {
	logger: yourLogger,                                  //Optional: Will automatically log exceptions	
	errorTransformer: alwaysBlameTheUserErrorTransformer //Optional: Lets you add status codes 
};

restifyPromise.install( server, options ); // Options is not required

//Async function, automatically calls send with the returned object and next
server.get('/lookup/:name', async function (req, res) {
	return await SomePromise.work( req.parms.name );
});

//Promise function
server.get('/echo/:name', function (req) {
	const params = req.params; 
	return Promise.resolve( { params } );
});

//Existing restify method
server.get('/echo/:name', function (req, res, next) {
	  res.send(req.params);
	  next();
});

server.listen(8080, function () {
	  console.log('%s listening at %s', server.name, server.url);
});
```


# Installation
```bash
$ npm install --save restify-await-promise
```

## Bugs

See <https://github.com/PhinCo/restify-await-promise/issues>.
