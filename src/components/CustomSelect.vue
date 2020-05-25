<template>
  <div
    class="custom-select"
  >
    <label
      class="custom-select__label"
      v-if="label !== ''"
      for="selectInput"
    >
      {{ label }}
    </label>
    <input
      class="custom-select__input"
      id="selectInput"
      :value="value"
      :placeholder="selectedValue"
      @click="toggleSelector"
      readonly
    >
    <div :class="calcDropdownClasses">
      <div
        v-for="(item, index) in items"
        :key="index"
        class="dropdown-item"
        @click="chooseDropdownItem(item)"
      >
        {{ itemData(item) }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CustomSelect',
  data () {
    return {
      isOpen: false,
      internalValue: this.value
    }
  },
  model: {
    prop: 'value',
    event: 'change'
  },
  props: {
    items: {
      type: Array
    },
    value: {
      type: [String, Object, Number]
    },
    customText: {
      type: String
    },
    customValue: {
      type: String
    },
    label: {
      type: String
    }
  },
  methods: {
    chooseDropdownItem (item) {
      const isObject = typeof (item) === 'object'
      this.$emit('change', isObject ? item[this.customValue] : item)
      this.isOpen = false
    },
    toggleSelector () {
      this.isOpen = !this.isOpen
    },
    itemData (item) {
      const isObject = typeof (item) !== 'object'
      return isObject ? item : item[this.customText]
    }
  },
  computed: {
    calcDropdownClasses () {
      return ['custom-select__dropdown', this.isOpen ? '_open' : '_close']
    },
    selectedValue () {
      return !this.internalValue ? 'Выберите из списка' : this.internalValue
    }
  }
}
</script>
