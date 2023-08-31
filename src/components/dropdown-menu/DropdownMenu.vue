<template>
  <div class="dropdown-container">
    <label class="dropdown-label">{{ label }}</label>
    <select class="dropdown" v-model="selectedItem" @change="handleSelection">
      <option :value="null" disabled>Select element</option>
      <option :value="'All'">All</option>
      <option v-for="item in data" :key="item.id" :value="item.value">{{ item.label }}</option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: () => ''
    },
    data: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      selectedItem: null
    }
  },
  methods: {
    handleSelection(event) {
      this.selectedItem = event.target.value

      let valueToEmit = this.selectedItem

      if (this.selectedItem === 'All') {
        valueToEmit = null
      }
      this.$emit('selection', valueToEmit)
    }
  },
  mounted() {
    this.selectedItem = null
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/styles/variables.scss';
.dropdown-container {
  display: flex;
  align-items: center;
  gap: 40px;
  flex-direction: row;
  width: 350px;
  margin: 0 auto;
}

.dropdown-label {
  width: 30px;
  font-size: 16px;
}

.dropdown {
  width: 100%;
  padding: 10px;
  font-size: 14px;
  border-radius: 5px;
  border: 1px solid #ccc;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  appearance: none;
  cursor: pointer;
  transition: border-color 0.3s ease;

  &:hover,
  &:focus {
    border-color: $primary-blue;
  }
}
</style>
