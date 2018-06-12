# Official Documents
[how to use as a plugin](https://nuxtjs.org/faq/google-analytics#how-to-use-google-analytics-)

[how to use as a nuxtjs/module](https://github.com/nuxt-community/analytics-module)

# Example

## when using plugin

1. plugins

```
export default({app}) => {
  if (process.env.NODE_ENV !== 'production) return
  
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
  
  ga('create', 'UA-XXXXXXXX-X', 'auto')
  
  app.router.afterEach((to, from) => {
    ga('set', 'page', to.fullPath)
    ga('send', 'pageview')
  })
}
```

2. nuxt.config.js

```
module.exports = {
  ...
  plugins: [
    { src: '~plugins/ga.js', ssr: false }
  ]
  ...
}
```

## when using google-analytics module

1. console

```
$ npm i @nuxtjs/google-analytics
```

2. nuxt.config.js

```
module.exports = {
  modules: [
    ['@nuxtjs/google-analytics', {
      id: 'UA-XXXXXXXX-X'
    }]
 ]
}
```

3. options

[vue-analytics](https://matteogabriele.gitbooks.io/vue-analytics/content/)
