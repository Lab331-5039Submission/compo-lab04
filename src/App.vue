<template>
  <div id="nav">
    <router-link :to="{ name: 'EventList', query: { perPage: 1 } }"
      >Home</router-link
    >
    | <router-link :to="{ name: 'About' }">About</router-link> |

    <router-link
      v-for="num in index"
      :key="num"
      :to="{ name: 'EventList', query: { perPage: num } }"
      >Show {{ num }} |</router-link
    >
  </div>

  <!-- new element -->
  <router-view />
</template>

<script>
import EventService from '@/services/EventService.js'
import { watchEffect } from '@vue/runtime-core'
export default {
  data() {
    return {
      totalEvents: 0,
      index: null
    }
  },
  created() {
    watchEffect(() => {
      EventService.getEvents(this.perPage, this.page)
        .then((response) => {
          this.totalEvents = response.headers['x-total-count']
          this.index = Array.from(
            { length: this.totalEvents / 2 },
            (_, i) => i + 1
          )
        })
        .catch((error) => {
          console.log(error)
        })
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
h4 {
  font-size: 20px;
}
</style>
