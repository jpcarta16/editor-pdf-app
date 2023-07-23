<template>
    <div class="toolbar-pdf">
        <div class="buttons-container">
            <!-- Image element button -->
            <button @click="openImageUpload" class="small-button" data-type="image">
                <input ref="imageInput" type="file" @change="onImageUpload" accept="image/*" style="display: none" />
                <img :src="getImageIcon('image')" alt="Image" />
            </button>
            <!-- Text element button -->
            <button @click="addElement('text')" class="small-button" data-type="text">
                <img :src="getImageIcon('text')" alt="Text" />
            </button>
            <!-- Table element button -->
            <button @click="addElement('table')" class="small-button" data-type="table">
                <img :src="getImageIcon('table')" alt="Table" />
            </button>
        </div>
        <!-- Iterate over each downloaded element -->
        <div v-for="(element, index) in downloadedElements" :key="index" class="element"
            @dragstart="onDragStartElement($event, element)" draggable="true" :data-type="element.type">
            <!-- Display the icon of each downloaded element -->
            <img :src="getImageIcon(element.type)" :alt="element.type" class="element-icon" />
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            draggingElement: null,
            downloadedElements: [],
            draggingElementIndex: null,
            imageIcon: require('@/assets/ImageIcon.png'),
            textIcon: require('@/assets/TextIcon.png'),
            tableIcon: require('@/assets/TableIcon.png'),
        };
    },
    methods: {
        // Get the icon image based on the element type
        getImageIcon(type) {
            switch (type) {
                case "ElementImage":
                    return this.imageIcon;
                case "ElementText":
                    return this.textIcon;
                case "ElementTable":
                    return this.tableIcon;
                default:
                    return null;
            }
        },
        // Open the image upload dialog
        openImageUpload() {
            this.$refs.imageInput.click();
        },
        // Method triggered when an image is uploaded
        onImageUpload(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    this.downloadedElements.push({
                        type: "ElementImage",
                        content: e.target.result,
                    });
                };
                reader.readAsDataURL(file);
            }
        },
        // Method triggered when an element is dragged from the toolbar
        onDragStartElement(event, element) {
            this.draggingElement = element;
            event.dataTransfer.setData("text/plain", JSON.stringify(element));
        },
        // Method triggered when an element is dragged from the canvas
        onDragStart(index) {
            this.draggingElementIndex = index;
        },
        // Method triggered when an element is dropped onto the canvas
        onDragEndCanvas() {
            this.draggingElementIndex = null;
        },
        // Add an element to the downloaded elements
        addElement(type) {
            switch (type) {
                case "image":
                    this.downloadedElements.push({
                        type: "ElementImage",
                        content: "https://example.com/image.jpg",
                    });
                    break;
                case "text":
                    this.downloadedElements.push({
                        type: "ElementText",
                        content: "Downloaded text",
                    });
                    break;
                case "table":
                    this.downloadedElements.push({
                        type: "ElementTable",
                        content: "Table data",
                    });
                    break;
                default:
                    break;
            }
        },
        // Method triggered when an element is clicked on the canvas
        onCanvasElementClick(index) {
            this.$emit("canvasElementClick", index);
        },
    },
};
</script>

<style scoped>
.elements-title {
    color: var(--secondary-secondary-400, #3A6B88);
    font-family: Montserrat;
    font-size: 18px;
    font-style: normal;
    font-weight: 600;
    line-height: normal;
    margin-bottom: 8px;
}

.element-icon {
    width: 30px;
    height: 30px;
}

.downloaded-elements {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 16px;
}

.canvas-elements {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 16px;
}

.element {
    width: 24px;
    height: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 8px;
}

.element.selected {
    border: 1px solid var(--primary-primary-200, #50ab84);
}

.toolbar-pdf {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 16px;
    color: white;
}

.buttons-container {
    display: flex;
    justify-content: flex-start;
    gap: 8px;
    margin-bottom: 8px;
}

.small-button {
    display: flex;
    width: 60px;
    height: 60px;
    justify-content: center;
    align-items: flex-start;
    flex-shrink: 0;
}

.small-button:last-child {
    margin-right: 0;
}

.small-button img {
    width: 100%;
    height: 100%;
}


.small-button[data-type="image"] img {
    content: url("@/assets/ImageIcon.png");
}

.small-button[data-type="text"] img {
    content: url("@/assets/TextIcon.png");
}

.small-button[data-type="table"] img {
    content: url("@/assets/TableIcon.png");
}
</style>
