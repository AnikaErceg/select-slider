<template>
  <div class="select-wrapper">
    <select :name="name" v-model="selected" @change="onSelect">
      <option v-for="element in elements" :key="element">{{ element }}</option>
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
        <div class="center-options">
          <!-- 
            for demo purposes, beginning point is hardcoded;
            for more bulletproof solution, beginning point should be calculated
            acording to parent width

            in this case, every next element is -100px from the previous one
            since elementa are positioned center in a box that's 100px wide
           -->
          <div class="options" :style="`transform: translate3d(${xPosition}px, 0px, 0px)`" @mousedown="onDragStart" @mousemove="onDrag" @mouseup="onDragStop">
            <div v-for="(element, index) in elements"
              :key="element"
              :data-index="index"
              :class="{option: true, 'default-selected': element === selected}">{{ element }}</div>
          </div>
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
      showSelect: false,
      isMouseDown: false,
      startingPoint: null,
      movedBy: null,
      translate3dstart: 250,
    };
  },
  computed: {
    xPosition() {
      return this.translate3dstart - this.movedBy
    }
  },
  methods: {
    onDragStart(e) {
      this.isMouseDown = true
      this.startingPoint = e.clientX
      console.log("mouse down", e.clientX)
    },
    onDrag(e) {
      // e.preventDefault();
      if(this.isMouseDown) {
        this.movedBy = this.startingPoint - e.clientX
        console.log("dragging x", e.clientX, this.movedBy)
      }
    },
    onDragStop(e) {
      // only works when mouse is put up withing bounds of the slider
      this.isMouseDown = false
      this.translate3dstart = this.translate3dstart - this.movedBy
      this.movedBy = 0
      console.log("mouse up", e)
    },
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
