<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App" :posts="posts"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import {onMounted, ref} from "vue";



export default {
  name: 'App',
  components: {
    HelloWorld
  },
  setup() {
    const posts = ref([])

    onMounted(() => {
      fetch('http://lc-laravel-graphql.test/graphql', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          query: `
        query {
          posts {
            data {
              id
              title
            }
          }
        }
      `
        })

      })
          .then(res => res.json())
          .then(result => {
            console.log(result)
            posts.value = result.data.posts.data
          })
    });

    return {
      posts
    }
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
  margin-top: 60px;
}
</style>
