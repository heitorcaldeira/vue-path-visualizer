<template>
  <div class="header">
    <div class="container">
      <div class="left">
        <h1>
          <img src="../../assets/images/logo.svg" width="32"/>
          Algorithm Path Visualizer in Vue 3
        </h1>
        <div>
          Choose and algorithm:
          <select v-model="algorithm">
            <option value="dijkstra">Dijkstra’s</option>
            <!--            <option value="a-search">A* Search</option>-->
            <!--            <option value="d-search">D*</option>-->
          </select>
          <button @click="onClickStart" :disabled="isRunning" type="button">Start</button>
          <button @click="$emit('clickReset')" :disabled="isRunning" type="button">Reset</button>
        </div>
        <p>Click and drag to create barriers :)</p>
      </div>
      <div class="right">
        <a href="https://github.com/heitorcaldeira">
          <img src="../../assets/images/GitHub-Mark-32px.png" border="0"/>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {isCompleted: Boolean},
  emits: ['clickStart', 'clickReset'],
  data: () => {
    return {
      isRunning: false,
      algorithm: 'dijkstra'
    }
  },
  watch: {
    isCompleted() {
      this.isRunning = false
    }
  },
  methods: {
    onClickStart() {
      this.isRunning = true;
      this.$emit('clickStart', this.algorithm);
    }
  }
}
</script>

<style lang="scss" scoped>
@import "src/assets/styles/imports/variables";

.header {
  background: $primary;
  padding: 10px;
  height: 120px;

  .container {
    display: flex;
    height: 100%;
    justify-content: space-between;
    align-items: center;
  }

  .right {
    display: flex;
    justify-content: center;
    flex-direction: column;
  }

  h1 {
    margin-top: 0;
    font-size: 22px;
  }
}

button {
  margin-left: 8px;
}

@media(max-width: $breakpoint-sm) {
  .right {
    display: none;
  }

  h1 {
    display: none;
  }
}

</style>
