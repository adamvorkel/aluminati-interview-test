<template>
  <div id="app">
    <input type="number" v-model="limit" />
    <!-- limit passed in via prop -->
    <Numbers :limit="parseInt(limit)"/>
  </div>
</template>

<script>
import Numbers from '@/components/Numbers';

export default {
  name: 'App',
  components: {Numbers},
  data()
  {
    return {
      limit: 100
    }
  }
}
</script>

<style scoped>
</style>
