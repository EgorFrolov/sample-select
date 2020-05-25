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
      :value="selectedValue"
      :placeholder="selectedValue"
      @click="toggleSelector"
      readonly
      :disabled="disabled"
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
      internalValue: '',
      internalText: ''
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
    },
    disabled: {
      type: Boolean,
      default: false
    },
    placeholder: {
      type: String,
      default: 'Выберите из списка'
    }
  },
  methods: {
    chooseDropdownItem (item) {
      const isObject = typeof (item) === 'object'
      this.internalValue = isObject ? item[this.customValue] : item
      this.internalText = isObject ? item[this.customText] : item
      this.$emit('change', this.internalValue)
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
      return !this.internalText ? this.placeholder : this.internalText
    }
  }
}
</script>
<style lang="scss" scoped>
  .custom-select {
    cursor: pointer;
    width: 200px;
    &__label {
      font-size: 14px;
    }

    &__input {
      margin-top: 10px;
      border-radius: 5px;
      border: 1px solid gray;
      line-height: 24px;
      font-size: 14px;
      padding: 5px;
      cursor: pointer;
    }

    &__dropdown {
      font-size: 20px;
      border-radius: 5px;
      padding: 10px;
    }

    .dropdown-item {
      padding: 10px;
      margin-top: 5px;
      border-radius: 5px;
    }
    .dropdown-item:hover {
      border: 1px solid gray;
    }
  }
  ._open {
    display: block;
  }
  ._close {
    display: none;
  }
</style>
