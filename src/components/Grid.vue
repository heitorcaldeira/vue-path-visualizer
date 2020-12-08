<script>
import {h} from 'vue';
import Node from './Node'
import {dijkstra, getNodesInShortestPathOrder} from "@/algorithms/dijkstra";

export default {
  name: 'Grid',
  props: {
    selectedAlgorithm: String
  },
  emits: ['searchCompleted'],
  data: function () {
    return {
      gridHeight: 0,
      isRunning: false,
      nodes: [],
      mousePressed: false,
      initialNode: null,
      finishNode: null,
      renderTime: new Date().getUTCMilliseconds()
    }
  },
  watch: {
    selectedAlgorithm() {
      this.startAlgorithm()
    }
  },
  components: {
    Node
  },
  mounted() {
    this.updateGrid();

    window.addEventListener('resize', () => {
      this.updateGrid();
    })
  },
  render() {
    return h('div', {id: 'grid', ref: 'grid'}, [
      this.nodes.map(row => {
        return h('div', null, [
          row.map((node, index) => h(Node, {
            ...node,
            key: index + '-' + this.renderTime,
            onNodeMouseEnter: this.handleMouseEnter,
            onNodeMouseDown: this.handleMouseDown,
            onNodeMouseUp: this.handleMouseUp
          }))
        ])
      })
    ])
  },
  methods: {
    startAlgorithm() {
      this.isRunning = true
      // todo: refactor to implement other algorithms
      const path = dijkstra(this.nodes, this.nodes[this.initialNode.col][this.initialNode.row], this.nodes[this.finishNode.col][this.finishNode.row]);
      const shortest = getNodesInShortestPathOrder(this.nodes[this.finishNode.col][this.finishNode.row]);
      this.animateDijkstra(path, shortest)
    },
    updateGrid() {
      this.renderTime = new Date().getUTCMilliseconds()
      this.gridHeight = this.getGridHeight()
      this.nodes = this.getGridNodes()
    },
    getGridHeight() {
      return this.$refs.grid ? this.$refs.grid.clientHeight : 0;
    },
    animateDijkstra(visitedNodesInOrder, nodesInShortestPathOrder) {
      for (let i = 0; i <= visitedNodesInOrder.length; i++) {
        if (i === visitedNodesInOrder.length) {
          setTimeout(() => {
            this.animateShortestPath(nodesInShortestPathOrder);
          }, 10 * i);
          return;
        }
        setTimeout(() => {
          const node = visitedNodesInOrder[i];
          document.getElementById(`node-${node.column}-${node.row}`).classList.add('node-visited');
        }, 10 * i);
      }
    },
    animateShortestPath(nodesInShortestPathOrder) {
      for (let i = 0; i < nodesInShortestPathOrder.length; i++) {
        setTimeout(() => {
          const node = nodesInShortestPathOrder[i];
          document.getElementById(`node-${node.column}-${node.row}`).classList.add('node-shortest-path');
        }, 50 * i);
      }
      this.isRunning = false
      this.$emit('searchCompleted', true)
    },
    getGridNodes() {
      const squareSize = 24
      const gridWidth = window.innerWidth;
      const gridHeight = this.gridHeight
      const totalColumns = Math.floor(gridWidth / squareSize)
      const totalRows = Math.floor(gridHeight / squareSize)
      this.initialNode = {row: Math.floor((gridHeight / squareSize) / 2), col: Math.floor(gridWidth / squareSize / 5)};
      this.finishNode = {
        row: Math.floor((gridHeight / squareSize) / 2),
        col: Math.floor((gridWidth / squareSize / 5) * 4)
      };
      const gridMap = []

      for (let i = 0; i < totalColumns; i++) {
        const rowMap = []
        for (let j = 0; j < totalRows; j++) {
          rowMap.push({...this.getNewNode(i, j)})
        }
        gridMap.push(rowMap)
      }

      return gridMap;
    },
    handleMouseDown(row, col) {
      if (!this.isRunning) {
        this.mousePressed = true
        this.handleMouseEnter(row, col)
      }
    },
    handleMouseEnter(row, col) {
      if (this.mousePressed) {
        const grid = [...this.nodes];
        const current = grid[row][col];
        const toggled = {...current, isDisabled: !current.isDisabled};
        grid[row][col] = toggled;

        this.nodes = grid;
      }
    },
    handleMouseUp() {
      this.mousePressed = false
    },
    getNewNode(row, col) {
      return {
        row: row,
        column: col,
        isStart: this.initialNode.row === col && this.initialNode.col === row,
        isFinish: this.finishNode.row === col && this.finishNode.col === row,
        isDisabled: false,
        isVisited: false,
        distance: Infinity,
        previousNode: null,
      }
    }
  }
}
</script>

<style lang="scss">
@import "../assets/styles/imports/variables";

#grid {
  background: $grey;
  height: calc(100% - 120px);
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
