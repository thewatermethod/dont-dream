<template>
    <div>
        {{ buildPoints }}
        <ul>
            <li v-for="attribute in attributes" :key="attribute.name">
                <label :for="attribute.name">{{ attribute.name }}</label>
                <input type="number" :max="max" min="-1" :name="attribute.name" :id="attribute.name"
                    v-model="attribute.value" />
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">

const props = defineProps<{
    buildPoints: number,
}>()

const { buildPoints } = props;

const max = computed(() => buildPoints - attributes.value.reduce((acc, curr) => {
    console.log(curr.value);
    return acc + curr.value;
}, 0));

const attributes = ref([
    {
        name: 'Body',
        value: 0,
    },
    {
        name: 'Library Use',
        value: 0,
    },
    {
        name: 'Affect',
        value: 0,
    },
    {
        name: 'Power',
        value: 0,
    }
])

</script>