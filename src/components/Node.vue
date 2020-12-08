<template>
  <div
      class="node"
      :id="'node-' + column + '-' + row"
      :class="classNames"
      @mouseenter="this.$emit('nodeMouseEnter', row, column)"
      @mousedown="this.$emit('nodeMouseDown', row, column)"
      @mouseup="this.$emit('nodeMouseUp')"
  >
  </div>
</template>

<script>
export default {
  name: 'Node',
  data: function() {
    return {
      classNames: {
        'disabled': this.isDisabled,
        'start': this.isStart,
        'finish': this.isFinish,
      }
    }
  },
  props: {
    row: Number,
    column: Number,
    isStart: Boolean,
    isFinish: Boolean,
    isDisabled: Boolean
  },
  watch: {
    isDisabled: function(value) {
      this.classNames = {
        ...this.classNames,
        'disabled': value,
      }
    }
  }
}
</script>

<style lang="scss">
@import "../assets/styles/imports/variables";
.node {
  width: 24px;
  height: 24px;
  border: .1px dashed rgba(0, 0, 0, .5);

  &.node-visited {
    animation-name: visitedAnimation;
    animation-duration: .5s;
    animation-timing-function: ease-out;
    animation-direction: alternate;
    animation-iteration-count: 1;
    animation-fill-mode: forwards;
    animation-play-state: running;
  }

  &.node-shortest-path {
    background: beige !important;
  }

  &.disabled {
    background: $secondary;
  }

  &.start {
    background: $white !important;

    &:after {
      content: ' \21A3';
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100%;
    }
  }

  &.finish {
    background: $black !important;

    &:after {
      content: ' \229D';
      display: flex;
      color: $white;
      justify-content: center;
      align-items: center;
      height: 100%;
    }
  }
}

@keyframes visitedAnimation {
  0% {
    transform: scale(0.3);
    background-color: $grey-darker;
    border-radius: 100%;
  }

  50% {
    background-color: $grey-light;
  }

  100% {
    transform: scale(1);
    background-color: $accent;
  }
}
</style>
