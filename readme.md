# Heroku Buildpack: NGINX + NODE.JS

Nginx-buildpack vendors NGINX inside a dyno and connects NGINX to an app server Node.js (no via UNIX domain sockets for now).

## Motivation

Read https://github.com/ryandotsmith/nginx-buildpack.git

## Versions

* Buildpack Version: 0.4
* NGINX Version: 1.5.7

## Requirements

* [Your webserver listens to the socket at `/tmp/nginx.socket`.] ?
* You touch `/tmp/app-initialized` when you are ready for traffic.
* [You can start your web server with a shell command.] ?

## Features

* Unified NXNG/App Server logs.
* [L2met](https://github.com/ryandotsmith/l2met) friendly NGINX log format.
* [Heroku request ids](https://devcenter.heroku.com/articles/http-request-id) embedded in NGINX logs.
* Crashes dyno if NGINX or App server crashes. Safety first.
* Language/App Server agnostic.
* Customizable NGINX config.
* Application coordinated dyno starts.

## Setup

Example: the app exists already.

**Create App**

$ cd workspace/appName
add fs.openSync('/tmp/app-initialized', 'w'); in app.js
$ git init
$ git add .
$ git commit -m "init"
$ heroku create (git remote -v)	
$ heroku apps:rename newAppName
$ git push heroku master (source: https://git.heroku.com/flashjob.git) or $ git push
$ heroku open
$ heroku logs --tail | grep -ve "method=HEAD"

**Nginx & Node.js**

$ heroku config:set BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git
$ echo 'https://github.com/heroku/heroku-buildpack-nodejs.git' >> .buildpacks
$ echo 'https://github.com/Gionni/nginx-buildpack.git' >> .buildpacks
Remember, you gotta work nginx.conf.erb out, first...
Update Procfile: web: bin/start-nginx ./node_modules/.bin/forever app.js
$ git add .
$ git commit -m 'Add multi-buildpack + Update procfile for NGINX buildpack'
$ git push heroku master or $ git push

## License
Copyright (c) 2013 Ryan R. Smith
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
