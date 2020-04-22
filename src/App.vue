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

      <div class="initial-view clickable" @click="onSelectEdit">
        <div class="title">{{selected}}</div>
        <div class="bottom">
          <div class="subtitle">€/Month</div>
          <button type="button">Edit</button>
        </div>
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
        "36",
        "40",
        "45",
        "50",
        "55",
        "60",
        "70",
        "75",
        "80",
        "434"
      ],
      selected: "50",
    }
  },
  methods: {
    onSelect(selected) {
      this.selected = selected
    },
    onSelectEdit() {
      console.log("editing...")
    }
  }
};
</script>

<style src="./assets/styles.css">
