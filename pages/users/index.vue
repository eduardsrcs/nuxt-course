<template>
  <section>
    <h1>{{pageTitle}}</h1>
    <ul>
      <li v-for="user of users" :key="user.id">
        <a href="#" @click.prevent="openUser(user.id)">{{user.name}} </a>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  async fetch({store}) {
    console.log(store.getters.users);
    if(store.getters['users/users'].length === 0){ // module/getter name
      await store.dispatch('users/fetch')
    }
  },
  data: ()=>({
    // users: [],
    pageTitle: 'Users page'
  }),
  computed: {
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
</script>
