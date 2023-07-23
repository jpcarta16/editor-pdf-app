<template>
    <div class="element-text">
        <!-- Display the text content -->
        <p>{{ content }}</p>
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
.element-text {
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

.element-text p {
    text-align: center;
    margin: 0;
}
</style>
