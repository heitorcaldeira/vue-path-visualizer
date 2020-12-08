<template>
  <Header
      @clickStart="onClickStart"
      @clickReset="$refs.child.updateGrid()"
      :isCompleted="isRunning"
  >
  </Header>
  <Grid
      @searchCompleted="onSearchComplete"
      :selectedAlgorithm="selectedAlgorithm"
      ref="child">
  </Grid>
</template>

<script>
import Header from "@/components/layout/Header";
import Grid from "@/components/Grid";

export default {
  name: 'App',
  components: {Grid, Header},
  data: function () {
    return {
      selectedAlgorithm: '',
      isRunning: false,
    }
  },
  methods: {
    onClickStart(algorithm) {
      this.selectedAlgorithm = algorithm;
      this.$refs.child.startAlgorithm();
    },
    onSearchComplete() {
      this.isRunning = !this.isRunning
    }
  }
}
</script>

<style lang="scss">

</style>
