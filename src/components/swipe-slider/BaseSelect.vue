<template>
  <div class="select-wrapper">
    <select :name="name" v-model="selected" @change="onSelect">
      <option v-for="element in elements" :key="element">{{element}}</option>
    </select>
    <div v-show="showTitleCard" class="initial-view clickable" @click="onSelectEdit">
      <div class="title">{{selected}}</div>
      <div class="bottom">
        <div class="subtitle">€/Month</div>
        <button type="button">Edit</button>
      </div>
    </div>

    <div v-show="showSelect">
      <div class="select-options">
        <div class="top-text">{{topText}}</div>
        <div class="options">
          
        </div>
        <div class="bottom-text">{{bottomText}}</div>
      </div>

      <!-- TODO
        item boxes with overflow and gradient on the sides
        add transformX style, which will be changed as user 
        drags the selection
        on touch start / on drag start
        
        set startting point to default element
        as new element is set, update native select
        hide native element
      -->
    </div>
  </div>
</template>

<script>
export default {
  name: "base-select",
  props: {
    name: {
      type: String,
      default: "swipe-select"
    },
    topText: {
      type: String
    },
    bottomText: {
      type: String
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
      showTitleCard: true,
      showSelect: false
    };
  },
  methods: {
    onSelect() {
      this.$emit("select", this.selected);
    },
    onSelectEdit() {
      this.showTitleCard = false;
      this.showSelect = true;
      console.log("editing...");
    }
  }
};
</script>
