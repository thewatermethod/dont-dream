<template>
    <div class="attribute">
        <p>{{ name }}</p>
        <div class="attribute-controls">
            <button @click.prevent="$emit('decreaseAttributeValue', name, value - 1)">-</button>
            <span class="attribute-value">{{ value }}</span>
            <button @click.prevent="$emit('increaseAttributeValue', name, value + 1)">+</button>
        </div>
    </div>
</template>

<script setup lang="ts">
const props = defineProps<{
    name: 'Library Use' | 'Body' | 'Affect' | 'Power',
    value: number,
}>()

const {
    name,
    value,
} = props;

</script>

<style scoped>
.attribute {
    border: 1px solid tomato;
    display: flex;
    flex-direction: column;
    padding: 1rem;
    text-align: center;
}

.attribute p {
    font-weight: bold;
}

.attribute div {
    align-items: center;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

.attribute span {
    margin: 0 1rem;
}

.attribute-controls button {
    font-size: 2rem;
    height: 2rem;
    width: 2rem;
}
</style>