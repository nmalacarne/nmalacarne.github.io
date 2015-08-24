---
layout: post
title: Setting up Node.js on Ubuntu 
---
*This guide assumes that you are using Ubuntu 14 or 15  (although other versions may work as well).*

---
### Introduction
[Node.js](https://nodejs.org/) is a JavaScript runtime environment created by Ryan Dahl ([among many other contributors](https://github.com/joyent/node/graphs/contributors)) which combines Google's [V8 JavaScript engine](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)), an event-loop, and a low-level I/O API.

Node.js allows us to execute JavaScript outside of the browser, which enables us to write anything from scripts on our desktop which automate mundane tasks to full-blown web applications (like this blog).

Before we get into Node.js, there are a few dependencies that you will need to install:
1. curl
2. build-essential
3. libssl-dev

You can install these dependencies via opening the Terminal (ctrl+alt+T) and typing:
```
$ sudo apt-get install curl build-essential libssl-dev
```
---
### Installing NVM
The first step in setting up Node.js locally is to, well, install it. Many Linux distributions (such as Ubuntu) allow you to install Node.js via the Software Manager. However, I prefer to use a tool called 'Node Version Manager', or [NVM](https://github.com/creationix/nvm) for short, to install and manage different versions of Node.js. 

We can install NVM via running:

`$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash`

This script will clone the NVM repository to `~/.nvm` and also add exports to your bash profile.

We can ensure that NVM has been installed correctly by running `nvm` in our terminal, which should result in something like the following:
```
Node Version Manager

...

Example:
  nvm install v0.10.32                  Install a specific version number
  nvm use 0.10                          Use the latest available 0.10.x release
  nvm run 0.10.32 app.js                Run app.js using node v0.10.32
  nvm exec 0.10.32 node app.js          Run `node app.js` with the PATH pointing to node v0.10.32
  nvm alias default 0.10.32             Set default node version on a shell

...
```
---

### Installing Node.js
Now that we have NVM installed, we can now install Node.js. For this tutorial, I will stick with the current version that is marked 'stable' on NVM. For a list of all available versions, just use `$ nvm ls-remote`.

We can install the current stable version via:
`$ nvm install stable`

Next, we want to set the default version that will be available whenever we start the Terminal:
`$ nvm alias default stable`

We can check which version our system is currently using via:
`$ which node`

Which should output something similiar to `/home/nmalacarne/.nvm/versions/node/v0.12.4/bin/node`

This is the path to our current Node.js runtime. The beauty of NVM is that Node.js packages and version are each stored in their own folder (vX.XX.X) in our `.nvm` folder. This helps isolate project dependencies to specific Node.js versions, which will come in handy when trying to troubleshoot problems.

---

### Our first Node.js Application
Now that Node.js has been installed, we can actually use it to do something fun. The first thing that we will do is create a directory for our project:
```
$ cd ~
$ mkdir -p projects/js/node/my-first-app
```
Let's create the only script for this exercise: `$ touch index.js`

We can run this script via `$ node index.js`, however, it currently does not do anything. So, go ahead and open `index.js` in your favorite text editor and add this line:

`console.log('Hello world!');`

Now save the file and try running it via `$ node index.js` again. You should see the text 'Hello world!' print to the console.

Right now you are probably unimpressed, so let's go ahead and create something a little more fun: a web server.

---

### Simple Web Server in Node.js
Node.js actually makes writing a web server insanely easy. Go ahead and open `index.js` again and replace the contents with:
```language-javascript
// include the http package from node core
var http = require('http');

// a simple request handler
function handleRequest(req, res) {
  res.end('Hello world!');
}

// create a server and use our request handler for every request
var server = http.createServer(handleRequest);

// spin up the server on port 3000
server.listen(3000, function() {
  console.log('Server is listening on: localhost:' + 3000);
});
```

Now, if you run `$ node index.js` again, you should see output indicating that the server is listening on port 3000 (you can stop this process via ctrl + c). If you open your browser and navigate to localhost:3000, you will see and very simple page with the text 'Hello world!'.

---

### Conclusion
I hope that I have shown you the power of Node.js. You can find the files related to this tutorial at [CraterDust's GitHub](https://github.com/CraterDust/node-simple-webserver). Check back in the future for more posts related to Node.js and Ubuntu.

 
