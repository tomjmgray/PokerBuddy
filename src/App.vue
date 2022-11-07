

<template>
  <header>



      <nav class="d-flex justify-content-center align-items-center py-4">
        <RouterLink to="/" class="light fs-20">Home</RouterLink>
        <RouterLink to="/handhelper" class="px-5 fs-20 light">Hand Helper</RouterLink>
      </nav>

  </header>

  <RouterView />
</template>


<script >
import { RouterLink, RouterView } from 'vue-router'

export default {
  data() {
    return {
      pants: 'Pants',
      shirt: 'Shirt'
    }
  }
}
</script>

<style scoped>
  nav {
    width: 100vw;
    margin: 0;
    display: flex;
    justify-content: space-evenly;
  }

</style>
