<template>
    <div>
        <!-- Conditionally render image or text based on the element type -->
        <img v-if="type === 'image'" :src="content" alt="Image" />
        <p v-if="type === 'text'">{{ content }}</p>
    </div>
</template>

<script>
export default {
    props: {
        type: String,
        content: String,
    },
};
</script>
