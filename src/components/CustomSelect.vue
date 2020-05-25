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
        :class="calcDropdownItemClasses(index)"
        @click="chooseDropdownItem(item, index)"
      >
        {{ itemData(item) }}
      </div>
    </div>
  </div>
</template>

<script>
import { KEY_DOWN_CODE, KEY_UP_CODE, KEY_ENTER_CODE } from '../common/constants'
export default {
  name: 'CustomSelect',
  data () {
    return {
      isOpen: false,
      internalValue: '',
      internalText: '',
      selectedIndex: 0
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
  mounted () {
    this.initKeyHandlers()
  },
  methods: {
    chooseDropdownItem (item, index) {
      const isObject = typeof (item) === 'object'
      this.internalValue = isObject ? item[this.customValue] : item
      this.internalText = isObject ? item[this.customText] : item
      this.$emit('change', this.internalValue)
      this.selectedIndex = index
      this.isOpen = false
    },
    toggleSelector () {
      this.isOpen = !this.isOpen
    },
    itemData (item) {
      const isObject = typeof (item) !== 'object'
      return isObject ? item : item[this.customText]
    },
    initKeyHandlers () {
      document.addEventListener('keyup', this.toggleSelectItem)
    },
    toggleSelectItem (event) {
      if (event.keyCode === KEY_UP_CODE && this.selectedIndex > 0) {
        this.selectedIndex--
      } else if (event.keyCode === KEY_DOWN_CODE && this.selectedIndex < this.items.length - 1) {
        this.selectedIndex++
      } else if (event.keyCode === KEY_ENTER_CODE && this.isOpen) {
        this.chooseDropdownItem(this.items[this.selectedIndex], this.selectedIndex)
      } else if (event.keyCode === KEY_ENTER_CODE && !this.isOpen) {
        this.isOpen = true
      }
    },
    calcDropdownItemClasses (index) {
      return ['dropdown-item', this.selectedIndex === index ? 'dropdown-item--selected' : '']
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
      &--selected {
        border: 1px solid gray;
      }
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
