<template>
  <div id="app">
    <!-- Toolbar Section -->
    <div class="container">
      <div class="toolbar-container">
        <!-- Elements Title -->
        <h4 class="toolbar-title">Elements</h4>
        <!-- Toolbar Component -->
        <ToolbarPdf @element-drag-start="onElementDragStart" />
      </div>

      <!-- Canvas Section -->
      <div class="canvas-container">
        <!-- Canvas Component -->
        <CanvasPdf :selectedElementIndex="selectedElementIndex"
          @element-removed-from-canvas="onElementRemovedFromCanvas" />
      </div>
    </div>
  </div>
</template>

<script>
import ToolbarPdf from "./components/ToolbarPdf.vue";
import CanvasPdf from "./components/CanvasPdf.vue";

export default {
  components: {
    ToolbarPdf,
    CanvasPdf,
  },
  data() {
    return {
      selectedElementIndex: null,
      downloadedElements: [],
    };
  },
  methods: {
    // Method triggered when an element is dragged from the toolbar
    onElementDragStart(index) {
      this.downloadedElements.push(this.$refs.toolbarPdf.downloadedElements[index]);
    },
    // Method triggered when an element is removed from the canvas
    onElementRemovedFromCanvas(element) {
      const index = this.downloadedElements.indexOf(element);
      if (index !== -1) {
        this.downloadedElements.splice(index, 1);
      }
    },
    // Method triggered when an element is clicked on the canvas
    onCanvasElementClick(index) {
      this.selectedElementIndex = index;
    },
  },
};
</script>

<style>
.toolbar-container {
  flex: 1;
  padding: 32px;
  display: flex;
  flex-direction: column;
}

.toolbar-title {
  color: var(--secondary-secondary-400, #3A6B88);
  font-family: Montserrat;
  font-size: 18px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
}

.canvas-container {
  flex: 1;
  padding: 32px;
  background-color: #fff;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f0f0f0;
}

.container {
  width: 100%;
  height: auto;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 15px solid #ccc;
  background-color: #f0f0f0;
}
</style>
