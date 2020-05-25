<template>
  <div id="app">
    <custom-select
      :items="items"
      v-model="selectedItem"
    >
    </custom-select>
  </div>
</template>

<script>
import CustomSelect from './components/CustomSelect'

export default {
  name: 'App',
  components: {
    CustomSelect
  },
  data () {
    return {
      items: [
        'test 1',
        'test 2',
        'test 3',
        'test 4'
      ],
      selectedItem: 'test 1'
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
