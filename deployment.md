//1.  create  dist/server.js 

var express = require('express')
var serveStatic = require('serve-static')
var compression = require('compression')
var port = process.env.PORT || 3000;
var domain =  process.env.DOMAIN;

function ensureDomain(req, res, next){
  if(!domain || req.hostname === domain){
    // OK, continue
    return next();
  };
  res.redirect('http://'+domain+req.url); // handle port numbers if you need non defaults
};


var app = express()
app.all('*', ensureDomain); // at top of routing calls
app.use(compression())
app.use(serveStatic(__dirname + '/public', {'extensions': ['html']}))
app.listen(port, function(){
  console.log('server running ...');
});

// 2. create dist/package.json
{
  "name": "mysite",
  "version": "0.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js"
  },
  "engines":{
    "node": "0.12.x"
  },
  "dependencies": {
    "compression": "^1.4.4",
    "express": "^4.12.3",
    "serve-static": "^1.9.2"
  }
}

// 3. run gulp build
// 4. for deploying push dist folder to heroku using git
