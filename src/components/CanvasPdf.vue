<template>
    <div class="canvas-pdf" ref="canvas" @dragover="onDragOver" @dragenter="onDragEnter" @dragleave="onDragLeave"
        @drop="onDropElement">
        <!-- Iterate over each canvas area -->
        <div v-for="(areaElements, area) in canvasAreas" :key="area" class="canvas-area" :data-area="area">
            <!-- Area Rectangle -->
            <div class="area-rectangle">
                <!-- Capitalize the first letter of the area name -->
                <p class="area-name">{{ `${capitalizeFirstLetter(area)}` }}</p>
            </div>
            <!-- Drag and drop text -->
            <p v-if="!hasElementsInArea(area)" class="drag-and-drop-text">Drag and drop an element within this area</p>
            <!-- Iterate over each element in the area -->
            <div v-for="(element, elementIndex) in areaElements" :key="elementIndex" class="element"
                @click="onElementClick(element)">
                <!-- Image element button -->
                <button v-if="element.type === 'ElementImage'" @click="onElementClick(element)" :data-type="element.type">
                    <img :src="require('@/assets/Image.png')" alt="Image" :style="{ width: '50px', height: '50px' }" />
                </button>
                <!-- Text element button -->
                <button v-else-if="element.type === 'ElementText'" @click="onElementClick(element)"
                    :data-type="element.type">
                    <img :src="require('@/assets/Text.png')" alt="Text" :style="{ width: '50px', height: '50px' }" />
                </button>
                <!-- Table element button -->
                <button v-else-if="element.type === 'ElementTable'" @click="onElementClick(element)"
                    :data-type="element.type">
                    <img :src="require('@/assets/Table.png')" alt="Table" :style="{ width: '50px', height: '50px' }" />
                </button>
            </div>
        </div>

        <!-- Iterate over each element in the canvas -->
        <div v-for="(element, index) in canvasElements" :key="index" class="element" draggable
            @dragstart="onDragStartCanvas(event, element)">
            <!-- Render the element component -->
            <component :is="element.type" :content="element.content" :style="{ width: '50px', height: '50px' }" />
        </div>
    </div>
</template>

<script>
import ElementImage from "./ElementImage.vue";
import ElementText from "./ElementText.vue";
import ElementTable from "./ElementTable.vue";

export default {
    components: {
        ElementImage,
        ElementText,
        ElementTable,
    },
    data() {
        return {
            draggingElement: null,
            canvasAreas: {
                header: [],
                body: [],
                footer: [],
            },
            canvasElements: [],
        };
    },
    methods: {
        // Check if an area has elements
        hasElementsInArea(area) {
            return this.canvasAreas[area].length > 0;
        },
        // Capitalize the first letter of a string
        capitalizeFirstLetter(str) {
            return str.charAt(0).toUpperCase() + str.slice(1);
        },
        // Method triggered when an element is dragged from the canvas
        onDragStartCanvas(event, element) {
            this.draggingElement = element;
            event.dataTransfer.setData("text/plain", JSON.stringify(element));
        },
        // Method triggered when an element is dropped onto the canvas
        onDropElement(event) {
            event.preventDefault();
            event.target.classList.remove("dragover");
            const elementData = event.dataTransfer.getData("text/plain");
            if (elementData) {
                const element = JSON.parse(elementData);
                const area = event.target.dataset.area;
                // Check if the element type is allowed in the area and add it to the canvas area
                if (area === "footer" && element.type !== "ElementText") {
                    return;
                }
                if (area === "header" && element.type !== "ElementImage") {
                    return;
                }
                if (area && area in this.canvasAreas) {
                    this.canvasAreas[area].push(element);
                    const canvasIndex = this.canvasElements.indexOf(element);
                    if (canvasIndex !== -1) {
                        this.canvasElements.splice(canvasIndex, 1);
                    }
                } else {
                    this.canvasAreas.body.push(element);
                    const canvasIndex = this.canvasElements.indexOf(element);
                    if (canvasIndex !== -1) {
                        this.canvasElements.splice(canvasIndex, 1);
                    }
                }
            }
        },
        // Method triggered when an element is dragged over the canvas
        onDragOver(event) {
            event.preventDefault();
            // Get the type of the element being dragged
            const type = event.dataTransfer.getData("text/plain");
            // Add visual styles based on the element type
            if (type === "ElementImage") {
                event.target.classList.add("dragover-image");
            } else if (type === "ElementText") {
                event.target.classList.add("dragover-text");
            } else if (type === "ElementTable") {
                event.target.classList.add("dragover-table");
            }
        },
        // Method triggered when an element is dragged into the canvas
        onDragEnter(event) {
            event.preventDefault();
            event.target.classList.add("dragover");
        },
        // Method triggered when an element is dragged out of the canvas
        onDragLeave(event) {
            event.preventDefault();
            event.target.classList.remove("dragover");
        },
        // Method triggered when an element is clicked on the canvas
        onElementClick(element) {
            const index = this.canvasElements.indexOf(element);
            if (index !== -1) {
                this.$emit("canvasElementClick", index);
            }
        },
    },
};
</script>

<style scoped>
.canvas-pdf {
    display: flex;
    width: 1054px;
    padding: 48px 110px;
    flex-direction: column;
    align-items: center;
    gap: 24px;
    align-self: stretch;
}

.canvas-area {
    display: flex;
    padding: 16px 8px;
    flex-direction: column;
    align-items: flex-start;
    gap: 24px;
    align-self: stretch;
    border: 1px dashed var(--secondary-secondary-400, #3a6b88);
    background-color: #3a6b88;
    color: white;
}

.drag-and-drop-text {
    text-align: center;
}


.area-rectangle {
    width: 200px;
    height: 40px;
    border: 2px dashed white;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    font-weight: bold;
}

.area-name {
    text-transform: capitalize;
}

.element {
    display: flex;
    padding: 64px 32px;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 40px;
    align-self: stretch;
    border-radius: 2px;
    border: 1px dashed var(--secondary-secondary-400, #3a6b88);
    background: var(--neutral-neutral-00, #fff);
}

.dragover {
    border: 2px dashed var(--primary-primary-200, #50ab84);
}

.dragover-image {
    border: 2px dashed var(--primary-primary-200, #50ab84);
}

.dragover-text {
    border: 2px dashed var(--tertiary-tertiary-400, #db5733);
}

.dragover-table {
    border: 2px dashed var(--secondary-secondary-400, #3a6b88);
}
</style>
