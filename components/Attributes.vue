<template>
    <div class="form-group">
        <ul class="attributes">
            <li v-for="attribute in attributeNames" :key="`attribute-${attributes[attribute]}`">
                <Attribute @increase-attribute-value="increaseAttributeValue"
                    @decrease-attribute-value="decreaseAttributeValue" :name="attribute" :value="attributes[attribute]" />
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">

let attributes = reactive({
    Body: 0,
    'Library Use': 0,
    Affect: 0,
    Power: 0,
}) as {
    Body: number,
    'Library Use': number,
    Affect: number,
    Power: number,
}

const totalAttributeUsage = computed(() => Object.values(attributes).reduce((a, b) => a + b, 0));
const attributeNames = Object.keys(attributes) as ('Body' | 'Library Use' | 'Affect' | 'Power')[];

function updateAttributeValue(
    name: 'Body' | 'Library Use' | 'Affect' | 'Power',
    newValue: number
) {
    attributes[name] = newValue;
}

function increaseAttributeValue(
    name: 'Body' | 'Library Use' | 'Affect' | 'Power',
    newValue: number
) {
    if (totalAttributeUsage.value < 3) {
        updateAttributeValue(name, newValue);
    }
}

function decreaseAttributeValue(
    name: 'Body' | 'Library Use' | 'Affect' | 'Power',
    newValue: number
) {
    if (attributes[name] > -1) {
        updateAttributeValue(name, newValue);
    }
}

</script>

<style scoped>
.attributes {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: space-between;
    list-style: none;
    padding: 0;
    margin: 0;
}

.attributes li {
    display: flex-item;
}
</style>