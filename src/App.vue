<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link> |
    <router-link to="HabitForm">Habits</router-link>
  </nav>
  <router-view/>
</template>
<script>
import HabitData from "@/components/HabitData.vue";
import HelloWorld from "@/components/HelloWorld.vue";
export default {
  name: 'App',
  components: {
    // eslint-disable-next-line vue/no-unused-components
    HabitData, HelloWorld
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

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
