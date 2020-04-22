<template>
  <div class="container">
    <div id="app" class="demo-box">
      <base-select
        name="select-one"
        :elements="elements"
        :default="selected"
        @select="onSelect"
      ></base-select>
      <div>
        Selected: {{selected}}
      </div>
    </div>
  </div>
</template>

<script>
import BaseSelect from "./components/swipe-slider/BaseSelect.vue"
export default {
  name: "App",
  components: {
    BaseSelect
  },
  data() {
    return {
      elements: [
        "one",
        "two",
        "three"
      ],
      selected: "two",
    }
  },
  methods: {
    onSelect(selected) {
      this.selected = selected
    }
  }
};
</script>

<style src="./assets/styles.css">
