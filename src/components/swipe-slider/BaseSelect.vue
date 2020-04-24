<template>
  <div class="select-wrapper">
    <select :name="name" v-model="currentSelected" v-show="false">
      <option v-for="element in elements" :key="element">{{ element }}</option>
    </select>
    <div v-show="showTitleCard"
        class="initial-view clickable"
        @click="onSelectEdit"
        tabindex="0"
        @keypress="keyPress"
    >
      <div class="title">{{selected}}</div>
      <div class="bottom">
        <div class="subtitle">€/Month</div>
        <button type="button">Edit</button>
      </div>
    </div>

    <div v-show="showSelect">
      <div class="select-options" tabindex="0" @keypress="keyPress" @keydown="keyPress">
        <div class="top-text"><span>{{topText}}</span></div>
        <div class="center-options">
          <!-- 
            for demo purposes, beginning point is hardcoded;
            for more bulletproof solution, beginning point should be calculated
            acording to parent width

            in this case, every next element is -100px from the previous one
            since elementa are positioned center in a box that's 100px wide
           -->
          <div :class="{options: true, 'dragging': isMouseDown}" :style="`transform: translate3d(${xPosition}px, 0px, 0px)`"
                @mousedown="onDragStart" @mousemove="onDrag" @mouseup="onDragStop"
          >
            <div v-for="(element, index) in elements"
              :key="element"
              :data-index="index"
              :class="{
                option: true,
                'default-selected': element === selected,
                'current-selected': element === currentSelected}">
                {{ element }}
            </div>
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
      maxPosition: null,
      currentSelected: this.default,
    };
  },
  computed: {
    xPosition() {
      let pos = this.translate3dstart - this.movedBy
      if(pos > 250) {
        pos = 250
      }
      if(pos < this.maxPosition) {
        pos = this.maxPosition
      }
      return pos
    }
  },
  mounted() {
    this.getStartingPosition()
    this.getMaxPosition()
  },
  methods: {
    onDragStart(e) {
      
      this.startingPoint = e.clientX
      setTimeout(() => {
        this.isMouseDown = true
      }, 50);
    },
    onDrag(e) {
      if(this.isMouseDown) {
        this.movedBy = this.startingPoint - e.clientX
      }
    },
    onDragStop() {
      this.translate3dstart = this.translate3dstart - this.movedBy

      if(this.translate3dstart > 250) this.translate3dstart = 250   

      // snapping to center of element
      // currently, for sake of demo, assums that the step between element centers is 100px
      // round number to nearest 100 and add 50 
      this.translate3dstart = Math.floor(this.translate3dstart/100)*100 + 50

      this.getCurrentPositionIndex(this.translate3dstart)
      
      // set new style to new selected element
      // change native select value accordingly
      this.movedBy = 0
     
      this.isMouseDown = false
    },
    keyPress(e) {
      if(e.key === "Enter") {
        this.showTitleCard = false;
        this.showSelect = true;
      }
      if (e.key === "ArrowRight") {
        this.translate3dstart = this.translate3dstart - 100
        this.getCurrentPositionIndex(this.translate3dstart)
      }
      if (e.key === "ArrowLeft") {
        this.translate3dstart = this.translate3dstart + 100 
        this.getCurrentPositionIndex(this.translate3dstart)
      }
    },
    onElemenyClick(i) {
      // events bubbled
      // traditional solutions I tried, didn't give desired effect
      if (!document.querySelector('.dragging')) {
        this.translate3dstart = 250 - i*100
        this.getCurrentPositionIndex(this.translate3dstart)
      }
    },
    onSelectEdit() {
      this.showTitleCard = false;
      this.showSelect = true;
    },
    getCurrentPositionIndex(pos) {
      let index = 0
      let temp = 250
      while(pos !== temp) {
        temp = temp - 100
        index = index + 1
      }
      this.currentSelected = this.elements[index]
      this.$emit("select", this.currentSelected);
    },
    getMaxPosition() {
      let index = this.elements.length - 1
      let pos = 250
      while(index) {
        pos = pos - 100
        index--
      }
      this.maxPosition = pos
    },
    getStartingPosition() {
      let index = this.elements.indexOf(this.default)
      let pos = 250
      while(index) {
        pos = pos - 100
        index--
      }
      this.translate3dstart = pos
    }
  }
};
</script>
