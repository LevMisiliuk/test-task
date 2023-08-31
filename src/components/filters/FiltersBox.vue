<template>
  <div class="filter-box">
    <div class="filter-box__item">
      <DropdownMenu
        :data="geners"
        label="Genre"
        @selection="value => handleSelection('genre_type', value)"
      />
    </div>
    <div class="filter-box__item">
      <DropdownMenu
        :data="rating"
        label="Rating"
        @selection="value => handleSelection('rating_score', value)"
      />
    </div>
    <div class="filter-box__item">
      <DropdownMenu
        :data="years"
        label="Year"
        @selection="value => handleSelection('year', value)"
      />
    </div>
  </div>
</template>

<script>
import DropdownMenu from '../dropdown-menu/DropdownMenu.vue'

export default {
  name: 'FiltersBox',
  components: { DropdownMenu },
  props: {
    geners: {
      type: Array,
      default: () => []
    },
    rating: {
      type: Array,
      default: () => []
    },
    years: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      selectedValues: {}
    }
  },
  methods: {
    handleSelection(field, value) {
      this.selectedValues[field] = value
      this.$emit('selection', this.selectedValues)
    }
  }
}
</script>

<style lang="scss" scoped>
.filter-box {
  width: 100%;
  display: flex;
  flex-direction: column;
  &__item {
    width: 100%;
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
}
</style>
