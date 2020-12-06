# Nuxt JS - Vue + SSR (быстрый курс за 70 минут)

[video lesson](https://www.youtube.com/watch?v=lm9olMCRCIc)

## Links

[nuxt docs installation page](https://nuxtjs.org/docs/2.x/get-started/installation)

[Vue cli home page](https://cli.vuejs.org)

[Vue jsinstallation page](https://v3.vuejs.org/guide/installation.html#release-notes)

## Intro

```bash
sudo npm i -g create-nuxt-app
create-nuxt-app nuxt-crash-course
```

🎉  Successfully created project nuxt-crash-course

  To get started:

```bash
cd nuxt-crash-course
npm run dev
```

  To build & start for production:

```bash
cd nuxt-crash-course
npm run build
npm run start
```
[time 2:00](https://www.youtube.com/watch?v=lm9olMCRCIc&t=120s)

so, running

```bash
npm run dev
```

launches **localhost:3000**

### Use VSCode Vetur extension!

## Package.json

### Scripts

in `package.json`

- generate &mdash; 
- build &mdash; creates static files in prod mode
- start &mdash; launches builded prod files
- generate &mdash; creates files in SPA mode

### Dependencies

- cross-env &mdash; sets variables for frontend and backend;
- nuxt &mdash; includes vuejs, vue-router and vuex

### DevDependencies

- nodemon

## nuxt.config.js

- mode: universal, that includes SSR, "spa" mode can be set also.
- head &mdash; includes base config, 
  - title &mdash; pkg.name, can be string.

## Folders

- assets &mdash; The `assets` directory contains your un-compiled assets such as Stylus or Sass files, images, or fonts.
- components &mdash; ...
- ...

## Include Bootstrap

```bash
npm i bootstrap
```

`nux.config.js` add this:

```js
  css: [
    '@/node_modules/bootstrap/dist/css/bootstrap.min.css'
  ],
```

`pages/index.vue`

```vue
<template>
  <div class="container">
    <h1>Hello nuxt</h1>
  </div>
</template>

<script>
export default {}
</script>

<style>

</style>
```

```bash
rm components/Logo.vue
```

then, if we look at rendered code. we'll see. that this is rendered on server side, that is optimal for SEO.

### Create Navbar

```bash
touch components/Navbar.vue
```

```vue
<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Nux SSR</a>

    <div class="collapse navbar-collapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active">
          <a class="nav-link" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">About</a>
        </li>
      </ul>
    </div>
  </nav>
</template>

```

go to `default` layout, remove styles.

```vue
<template>
  <div>
    <Navbar />
    <main>
      <div class="container">

        <nuxt />
      </div>
    </main>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar'

export default {
  components: [
    Navbar
  ]
}
</script>

<style>

</style>

```

nuxt is same as vue-router.

## Routing

here are no routing config, we should create components in *pages* folder and then...

```bash
touch pages/about.vue
```

```vue
<template>
  <section>
    <h1>About page</h1>
    <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Impedit ex aliquam laboriosam numquam soluta earum asperiores quasi, dolores voluptatem, sed nisi at consequatur sequi, expedita deleniti iure qui repudiandae culpa!</p>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolor, optio impedit dolorum sunt dolorem iure iusto quo accusantium vero aperiam aut nisi maxime nesciunt laudantium beatae voluptatum sequi delectus! Tempora?</p>
    <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sint, minima!</p>
  </section>
</template>

```

[time 23:00](https://www.youtube.com/watch?v=lm9olMCRCIc&t=1380s)

`Navbar.vue`

```vue
<ul class="navbar-nav mr-auto">
  <li class="nav-item active">
    <nuxt-link class="nav-link" to="/">Home</nuxt-link>
  </li>
  <li class="nav-item">
    <nuxt-link class="nav-link" to="/about">About</nuxt-link>
  </li>
</ul>
```

### Highlight link in Navbar

add this ti **nuxt-link**:

```vue
<nuxt-link exact active-class="active" class="nav-link" to="/">Home</nuxt-link>
<nuxt-link active-class="active" class="nav-link" to="/about">About</nuxt-link>
```

exact in first link highlights this link only when route exactly equals to "to" attribute.

### Optimization

Nuxt separates JS files into chunks to provide **lazy loading**. But it loads all chunks according ti their links . Add **no-prefetch** attribute into nuxt-link.

```vue
<nuxt-link exact no-prefetch active-class="active" class="nav-link" to="/">Home</nuxt-link>
```

## Creating dinamic routes

```bash
mkdir pages/users
touch pages/users/index.vue
```

```vue
<template>
  <section>
    <h1>Users page</h1>
    <ul>
      <li v-for="user of 5" :key="user">User {{user}}</li>
    </ul>
  </section>
</template>
```

add to `Navbar`:

```vue
<li class="nav-item">
  <nuxt-link active-class="active" class="nav-link" to="/users">Users</nuxt-link>
</li>
```

### Create links for each user

update `pages/users/index.vue`:

```vue
<template>
  <section>
    <h1>Users page</h1>
    <ul>
      <li v-for="user of 5" :key="user">
        <a href="#" @click.prevent="openUser(user)"> User {{user}} </a>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  methods: {
    openUser(user) {
      console.log(user);
      this.$router.push('/users/' + user)
    }
  }
}
</script>
```

## Create dynamic route

```bash
touch pages/users/_id.vue
```

```vue
<template>
  <section>
    <h1>User {{$route.params.id}}</h1>
  </section>
</template>
```

### Add validation

add script to `pages/users/_id.vue`

```vue
<script>
export default {
  validate({params}) {
    console.log('validate')
    return /^\d+$/.test(params.id)
  }
}
</script>
```

**validate** method receives *context* as parameter, where we take *params* from it.

params are from route, but this is not accessible, because this is rendered on server.

this validates something. Note, that if we go into this page firsttime, this validation occurs on server side, further validations are provided on client side.

if **params.id** is not number, 404 will be returned.

## Creating 404 layout

```bash
touch layouts/error.vue
```

```vue
<template>
  <section>
    <h1>404</h1>
	<p>Page not found.</p>
    <nuxt-link to="/">Home</nuxt-link>
  </section>
</template>

<style scoped>
section{
  width: 600px;
  margin: 0 auto;
  padding-top: 5rem;
}

h1{
  color: red;
}
</style>

```

## Create login layout

add to `Navbar`

```vue
<li class="nav-item">
  <nuxt-link active-class="active" class="nav-link" to="/login">Login</nuxt-link>
</li>
```

```bash
touch pages/login.vue
```

```vue
<template>
  <section>
    <form>
      <h1>Login page</h1>
      <div class="form-group">
        <input type="text" class="form-control">
      </div>
      <button class="btn btn-primary" type="submit">Login</button>
    </form>

    <p>
      <nuxt-link to="/">To home page</nuxt-link>
    </p>
  </section>
</template>

<style scoped>
form{
  width: 500px;
  margin: 0 auto;
}
</style>

```

But we need to create new layout for this page.

```bash
touch layouts/empty.vue
```

```vue
<template>
  <div class="empty-layout">
    <nuxt />
  </div>
</template>

<style scoped>
  .empty-layout{
    display: flex;
    justify-content: center;
    padding-top: 5rem;
  }
</style>

```

But this layout is not attached to our `login` page. Add layout field in script section:

```vue
<script>
export default {
  layout: ()=> 'empty'
}
</script>
```

## Nuxt modules

google this:

```
nuxt community awesome nuxt
```

go to [first search result on Github](https://github.com/nuxt-community/awesome-nuxt)

### Axios

scroll to **Modules** section and selsct Axios, this module redirects to [nuxt axios homepage](https://axios.nuxtjs.org).  In setup copy and run

```bash
npm install @nuxtjs/axios
```

check this module in `package.json`. 

in `nuxt.config.js` add record in modules section:

```js
'@nuxtjs/axios'
```

OK, this done...

in `pages/users/index` add

```vue
<li v-for="user of users" :key="user.id">
  <a href="#" @click.prevent="openUser(user.id)"> User {{user.name}} </a>
</li>
...
<script>
export default {
  data: ()=>({
    users: []
  }),
  async mounted() {
    this.users = await this.$axios.$get('https://jsonplaceholder.typicode.com/users')
  },
  methods: {
    openUser(user) {
      this.$router.push('/users/' + user)
    }
  }
}
</script>
```

But this solution will not provide SSR, because of AJAX with axios. To provide this render, use **asyncData** method

```js
async asyncData({$axios}){
  const users = await $axios.$get('https://jsonplaceholder.typicode.com/users')
  return {users}
},
```

so, *mounted* method can be removed.

This provides SSR ul with all users list items.

Nux merges objects in **asyncData** and **data** sections. So, in **data** section we can omit users field.

Let's update `_id` page:

```vue
<template>
  <section>
    <h1>{{user.name}}</h1>
    <hr>
    {{user.email}}
  </section>
</template>

<script>
export default {
  async asyncData({$axios, params}){
    const user = await $axios.$get('https://jsonplaceholder.typicode.com/users/'+ params.id)
    return {user}
  },
  validate({params}) {
    console.log('validate')
    return /^\d+$/.test(params.id)
  }
}
</script>

```

## Vuex

So, users can be stored in state. In `store` folder we can create file which can be either module or main file to vuex.

```bash
touch store/users.js
```

```js
export const state = () => {
  users: []
}

export const mutations = {
  setUsers(state, users){
    state.users = users
  }
}

export const actions = {
  async fetch({commit}) {
    const users = await this.$axios.$get('https://jsonplaceholder.typicode.com/users')
    commit('setUsers', users)
  }
}

export const getters = {
  users: s => s.users
}

```

from `pages/users/index.vue` remove **asyncData** section. 

time 57:00

Insert this:

```js
export default {
  async fetch({store}) {
    if(store.getters['users/users'].length === 0){ // module/getter name
      await store.dispatch('users/fetch')
    }
  },
  data: ()=>({
    // users: [],
    pageTitle: 'Users page'
  }),
  computed(){
    users() {
      return this.$store.getters['users/users']
    }
  },
  methods: {
    openUser(user) {
      this.$router.push('/users/' + user)
    }
  }
}
```

At this point we'll get an error of TypeError: Cannot read property 'length' of undefined. Just rebuild solution.

## Loading progress bar

[time 1:00:15](https://www.youtube.com/watch?v=lm9olMCRCIc&t=3615s)

in `nuxt.config` change:

```js
 /*
  ** Customize the progress-bar color
  */
  loading: {
    color: 'blue'
  }
```

## Securing routes

### Login

```bash
touch store/index.js
```

```js
export const state = () => ({
  token: null
})

export const mutations = {
  setToken(state, token){
    state.token = token
  },
  clearToken(state) {
    state.token = null
  }
}

export const actions = {
  login({commit}) {
    commit('setToken', 'truetoken')
  },
  logoutgin({commit}) {
    commit('clearToken')
  }
}

export const getters = {
  hasToken: s => !!s.token
}

```

in `login`

```vue
<template>
  <section>
    <form @submit.prevent="onSubmit">
      <h1>Login page</h1>
      <div class="form-group">
        <input type="text" class="form-control">
      </div>
      <p>
        <nuxt-link to="/">To home page</nuxt-link>
      </p>
      <button class="btn btn-primary" type="submit">Login</button>
    </form>

  </section>
</template>

<script>
export default {
  layout: ()=> 'empty',
  methods: {
    onSubmit() {
      this.$store.dispatch('login')
      this.$router.push('/')
    }
  }
}

</script>

<style scoped>
form{
  width: 500px;
  margin: 0 auto;
}
</style>

```

### Logout

`Navbar`

```vue
<template>
  <nav class="navbar navbar-expand-sm navbar-light bg-light">
    <a class="navbar-brand" href="#">Nuxt SSR</a>

    <div class="collapse navbar-collapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <nuxt-link exact no-prefetch active-class="active" class="nav-link" to="/">Home</nuxt-link>
        </li>
        <li class="nav-item">
          <nuxt-link active-class="active" class="nav-link" to="/about">About</nuxt-link>
        </li>
        <li class="nav-item">
          <nuxt-link active-class="active" class="nav-link" to="/users">Users</nuxt-link>
        </li>
        <li class="nav-item" v-if="!hasToken">
          <nuxt-link active-class="active" class="nav-link" to="/login">Login</nuxt-link>
        </li>
        <li class="nav-item" v-else>
          <a @click.prevent="logout" class="nav-link" href="#">Logout</a>
        </li>
      </ul>

    </div>
  </nav>
</template>

<script>
export default {
  computed: {
    hasToken() {
      return this.$store.getters.hasToken
    }
  },
  methods: {
    logout() {
      this.$store.dispatch('logout')
      this.$router.push('/login')
    }
  }
}
</script>

```

### Creating page protection

create middleware:

```bash
touch middleware/auth.js
```

```js
export default function({store, redirect}) {
  if(!store.getters.hasToken) {
    redirect('/login?message=login')
  }
}

```

3900

[time](https://www.youtube.com/watch?v=lm9olMCRCIc&t=3900s)

We want protect `about` page, so, we need to add code to this:

```vue
<script>
export default {
  middleware: ['auth']
}
</script>
```

and use query parameter in `login`:

```vue
<section>
    <div v-if="$route.query.message" class="alert alert-danger mb-3">
      Need login first
    </div>
    <form @submit.prevent="onSubmit">
...
```



quite simple!

### Some error appeared...

vuex.esm.js?2f62:497 [vuex] unknown action type: logout

## While starting app...

`store/index` add action:

```js
nuxtServerInit({dispatch}) {
  console.log('nuxtServerInit');
},
```

## Done!



