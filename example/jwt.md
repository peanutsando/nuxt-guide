### There are two ways to configure the auth module using "jwt".

1. Official auth-module

If you want to implement complex authentication flows, for example OAuth2, we suggest using the official auth-module
- setup

install with npm
```
npm install @nuxtjs/auth @nuxtjs/axios
```
edit nuxt.config.js
```
modules: [
  '@nuxtjs/axios',
  '@nuxtjs/auth'
],
​
auth: {
  // Options
}
```
- middleware

You can enable auth middleware either globally or per route. When this middleware is enabled on a route and loggedIn is false user will be redirected to redirect.login route. (/login by default)
1. set route
```
export default {
  middleware: 'auth'
}
```
2. set nuxt.config
```
router: {
  middleware: ['auth']
}
```
* In case of global usage, You can set auth option to false in a specific component and the middleware will ignore that route.
```
export default {
  auth: false
}
```

3. [Options](https://auth.nuxtjs.org/getting-started/options)
4. [Reference Schemes](https://auth.nuxtjs.org/reference/schemes)
- [Schemes local](https://auth.nuxtjs.org/reference/schemes/local)
- [Schemes Oauth2](https://auth.nuxtjs.org/reference/schemes/oauth2)
5. [API Auth](https://auth.nuxtjs.org/api/auth)
6. [API Storage](https://auth.nuxtjs.org/api/storage)
7. plugins

plugins/auth.js
```
export default function ({ app }) {
  if (!app.$auth.loggedIn) {
    return
  }

  const username = app.$auth.user.username
}
```
nuxt.config.js
```
{
  modules: {
    '@nuxtjs/auth'
  },
  auth: {
    plugins: [ '~/plugins/auth.js' ]
  }
}
```
---
2. Structure
- SSR(Server Side Rendering)

We should save the token in session browser cookie after login, then it can be accessed through ```req.headers.cookie``` in middleware files, ```nuxtServerInit``` function or wherever you can access the ```req```.

- CSR(Client Side Rendering)

1. install dependency
```
npm install js-cookie -save
npm install cookieparser -save
```

2. set login.vue file's script
```
const Cookie = process.client ? require('js-cookie') : undefined

export default {
  middleware: 'notAuthenticated',
  methods: {
    postLogin() {
      setTimeout(() => { // we simulate the async request with timeout.
        const auth = {
          accessToken: '10293poiuytrewqlkjhgfdsamnbvcxz84756'
        }
        this.$store.commit('setAuth', auth) // mutating to store for client rendering
        Cookie.set('auth', auth) // saving token in cookie for server rendering
        this.$router.push('/')
      }, 1000)
    }
  }
}
```

3. set store/index.js

```
import Vuex from 'vuex'

const cookieparser = process.server ? require('cookieparser') : undefined

const createStore = () => {
  return new Vuex.Store({
    state: () => ({
      auth: null
    }),
    mutations: {
      setAuth(state, auth) {
        state.auth = auth
      }
    },
    actions: {
      nuxtServerInit({ commit }, { req }) {
        let auth = null
        if (req.headers.cookie) {
          const parsed = cookieparser.parse(req.headers.cookie)
          try {
            auth = JSON.parse(parsed.auth)
          } catch (err) {
            // No valid cookie found
          }
        }
        commit('setAuth', auth)
      }
    }
  })
}

export default createStore
```

4. check auth middlewares
- auth.js
```
export default function ({ store, redirect }) {
  // If the user is not authenticated
  if (!store.state.auth) {
    return redirect('/login')
  }
}
```
- notAuth.js
```
export default function ({ store, redirect }) {
  // If the user is authenticated redirect to home page
  if (store.state.auth) {
    return redirect('/')
  }
}
```

5. set logout
```
const Cookie = process.client ? require('js-cookie') : undefined

export default {
  methods: {
    logout() {
      // Code will also be required to invalidate the JWT Cookie on external API
      Cookie.remove('auth')
      this.$store.commit('setAuth', null)
    }
  }
}
```
