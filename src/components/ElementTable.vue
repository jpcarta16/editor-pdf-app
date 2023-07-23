<template>
    <div class="element-table">
        <table>
            <!-- Iterate over each row of the table data -->
            <tr v-for="(row, index) in tableData" :key="index">
                <!-- Iterate over each cell of the row -->
                <td v-for="(item, itemIndex) in row" :key="itemIndex">{{ item }}</td>
            </tr>
        </table>
    </div>
</template>

<script>
export default {
    props: {
        content: {
            type: Array,
            default: () => [],
        },
    },
    computed: {
        // Process table data and ensure it's an array or provide a default value
        tableData() {
            return this.content.length > 0 ? this.content : [["", ""]];
        },
    },
};
</script>

<style scoped>
.element-table {
    display: flex;
    width: 250px;
    height: 250px;
    padding: 24px;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 8px;
    flex-shrink: 0;
    border-radius: 4px;
    border: 1px solid var(--tertiary-tertiary-400, #DB5733);
    background: var(--neutral-neutral-00, #FFF);
    box-shadow: 0px 2px 6px 0px rgba(0, 0, 0, 0.13);
}

.element-table table {
    width: 100%;
    height: 100%;
    border-collapse: collapse;
}

.element-table td {
    border: 1px solid var(--tertiary-tertiary-400, #DB5733);
    padding: 8px;
    text-align: center;
}
</style>
