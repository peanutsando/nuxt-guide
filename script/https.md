### Nuxt 2.0 will support https out of the box so proxy module will work also out of the box

#### Use in 2.0 or less

1. server.js
```
const { Nuxt, Builder } = require('nuxt')

const https = require('https')
const fs = require('fs')
const isProd = (process.env.NODE_ENV === 'production')
const port = process.env.PORT || 3000

// We instantiate nuxt.js with the options
const config = require('./nuxt.config.js')
config.dev = !isProd
const nuxt = new Nuxt(config)

// Render every route with Nuxt.js

// Build only in dev mode with hot-reloading
if (config.dev) {
  new Builder(nuxt).build()
    .then(listen)
    .catch((error) => {
      console.error(error)
      process.exit(1)
    })
}
else {
  listen()
}

function listen() {

  const options = {
    key: fs.readFileSync('/home/store/certs/store.key'),
    cert: fs.readFileSync('/home/store/certs/store.crt')
  };

  https
    .createServer(options, nuxt.render)
    .listen(port)
  console.log('Server listening on `localhost:' + port + '`.')
}
```

2. package.json
```
...
"scripts": {
  "dev": "node server.js",
  "build": "nuxt build",
  "start": "node server.js" // set environment variables to Production
}
...
```
