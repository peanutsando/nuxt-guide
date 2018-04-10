# Official Documents
[vue-i18n](http://kazupon.github.io/vue-i18n/en/)

# Example

1. plugins

```
import Vue from 'vue'
import VueI18n from 'vue-i18n'

Vue.use(VueI18n)

export default ({ app, store }) => {
  // Set i18n instance on app

  // Set the default language to Korean, check and change the browser language
  app.i18n = new VueI18n({
    locale: store.state.locale,
    fallbackLocale: 'ko',
    messages: {
      'ko': require('~/locales/ko.json'),
      'en': require('~/locales/en.json')
    }
  })

  app.i18n.path = (link) => {
    if (app.i18n.locale === app.i18n.fallbackLocale) {
      return `/${link}`
    }

    return `/${app.i18n.locale}/${link}`
  }
}
```

2. Language json file

```
{
  "ko": {
    "message": {
      "title": "주 제목",
      "subtitle": "부 내용"
    }
  },
  "en": {
    "message": {
      "title": "title",
      "subtitle": "sub title"
    }
  }
}
```
Or you can manage it with individual files.

3. vuex-store

```
export const state = () => ({
  locales: ['en', 'fr'],
  locale: 'en'
})

export const mutations = {
  SET_LANG(state, locale) {
    if (state.locales.indexOf(locale) !== -1) {
      state.locale = locale
    }
  }
}
```

4. middleware

```
export default function ({ isHMR, app, store, route, params, error, redirect }) {
  const defaultLocale = app.i18n.fallbackLocale
  // If middleware is called from hot module replacement, ignore it
  if (isHMR) return
  // Get locale from params
  const locale = params.lang || defaultLocale
  if (store.state.locales.indexOf(locale) === -1) {
    return error({ message: 'This page could not be found.', statusCode: 404 })
  }
  // Set locale
  store.commit('SET_LANG', locale)
  app.i18n.locale = store.state.locale
  // If route is /<defaultLocale>/... -> redirect to /...
  if (locale === defaultLocale && route.fullPath.indexOf('/' + defaultLocale) === 0) {
    const toReplace = '^/' + defaultLocale
    const re = new RegExp(toReplace)
    return redirect(
      route.fullPath.replace(re, '/')
    )
  }
}
```

5. nuxt.config.js

```
export default {
  loading: { color: 'cyan' },
  router: {
    middleware: 'i18n'
  },
  plugins: ['~/plugins/i18n.js'],
}
```

6. vue

```
<template>
  <div id="app">
    <div class="container">
      <h1>{{ $t('message.title') }}</h1>
      <p>{{ $t('message.subtitle') }}</p>
    </div>

    <nav>
      <ul>
        <li @click="changeLanguage('ko')">한국어</li>
        <li @click="changeLanguage('en')">영어</li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  methods: {
    changeLanguage(lang) {
      this.$i18n.locale = lang
    }
  }
}
</script>
```
