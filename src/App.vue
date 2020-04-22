<template>
  <div id="app">
    <base-test
      name="select-one"
      :elements="elements"
      :default="selected"
      @select="onSelect"
    ></base-test>
    <div>
      Selected: {{selected}}
    </div>
  </div>
</template>

<script>
import BaseTest from "./components/swipe-slider/BaseTest.vue"
export default {
  name: "App",
  components: {
    BaseTest
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
