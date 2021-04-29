# Admin Layout

## Requirements

### Lib
- bootstrap-vue
- vue-mq
- vue-router
- vue-i18n
- axios
- vue-toast-notification
- @fortawesome/fontawesome-svg-core,
- @fortawesome/free-solid-svg-icons,
- @fortawesome/vue-fontawesome,

### Must be implemented in application

#### store

```js
/**
 * @return {{
 *   name: String,
 * }}
 */
['Auth/getUser'] => {}

/**
 * @return void
 */
['Auth/logout'] => {}

/**
 * @return void
 */
['Auth/login'] => {}

/**
 * @return boolean
 */
['Auth/isLoggedIn'] => {}

/**
 * @param {String} permission
 * @return boolean
 */
['Auth/$can(permission'] => {}
```

#### app.js config
- dashboardName -> not required
- currentVersion -> not required
- vue router route with name 'login'
- menu
```js
[
  {
    to: '/profile', // required
    linkText: 'menu', // required
    permission: 'PERMISSION_USER_ADMIN',
  }
]
```
- mq

must define mobileMenu

```js
Vue.use(VueMq, {
  breakpoints: {
    sm: 576,
    md: 768,
    lg: 992,
    mobileMenu: 1200,
    xl: Infinity,
  },
})
```

#### global event handlers

this.$root.$emit('showMessageSuccess', message)

this.$root.$emit('showMessageFailure', message)


## Configuration

### Router config
Authorization middleware in `router.beforeEach({})` - https://router.vuejs.org/guide/advanced/meta.html
If not login, redirect after init
