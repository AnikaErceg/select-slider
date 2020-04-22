<template>
  <div>
    <select :name="name" v-model="selected" @change="onSelect">
      <option
        v-for="element in elements"
        :key="element">
        {{element}}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: "base-test",
  props: {
    name: {
      type: String,
      default: "swipe-select"
    },
    elements: {
      type: Array
    },
    default: {
      type: String
    },
    classes: {
      type: Object
    }
  },
  data() {
    return {
      selected: this.default,
    }
  },
  methods: {
    onSelect() {
      this.$emit("select", this.selected)
    }
  }
};
</script>
