<template>
    <div class="element-image">
        <!-- Display the image content -->
        <img :src="content" alt="Element Image" />
    </div>
</template>

<script>
export default {
    props: {
        content: {
            type: String,
            default: "",
        },
    },
};
</script>

<style scoped>
.element-image {
    display: flex;
    width: 110px;
    height: 110px;
    padding: 24px;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 8px;
    flex-shrink: 0;
    border-radius: 4px;
    border: 1px solid var(--neutral-neutral-200, #E9E9E9);
    background: var(--neutral-neutral-00, #FFF);
    box-shadow: 0px 2px 6px 0px rgba(0, 0, 0, 0.13);
}

.element-image img {
    width: 100%;
    height: 100%;
    border-radius: 4px;
}
</style>
