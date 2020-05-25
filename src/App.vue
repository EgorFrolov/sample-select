<template>
  <div id="app">
    <custom-select
      :items="items"
      label="Выпадающий список"
      custom-text="label"
      custom-value="value"
      :disabled="true"
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
        {
          label: 'Первый',
          value: 'one'
        },
        {
          label: 'Второй',
          value: 'two'
        },
        {
          label: 'Третий',
          value: 'three'
        },
        {
          label: 'Четвертый',
          value: 'four'
        }
      ],
      selectedItem: 'four'
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
