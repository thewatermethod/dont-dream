<template>
    <div class="form-group form-group__bond">
        <label :for="`bond-name-${bondGuid}`">Name</label>
        <input type="text" :name="`bond-name-${bondGuid}`" :id="`bond-name-${index}`" />

        <div class="bond-quality">
            <input type="checkbox" v-model="bondQuality" value="bond-quality-1" :name="`bond-quality-${index}`"
                :id="`bond-quality-1-${index}`" :disabled="oneDisabled" />
            <label name="bn" :for="`bond-quality-1-${index}`">
                <span class="sr-only">
                    Bond {{ index + 1 }} Quality 1
                </span>
            </label>
        </div>
        <div class="bond-quality">
            <input type="checkbox" v-model="bondQuality" value="bond-quality-2" :name="`bond-quality-${index}`"
                :id="`bond-quality-2-${index}`" :disabled="twoDisabled" />
            <label :for="`bond-quality-2-${index}`">
                <span class="sr-only">
                    Bond {{ index + 1 }} Quality 2
                </span>
            </label>
        </div>
        <div class="bond-quality">
            <input type="checkbox" v-model="bondQuality" value="bond-quality-3" :name="`bond-quality-${index}`"
                :id="`bond-quality-3-${index}`" :disabled="threeDisabled" />
            <label :for="`bond-quality-3-${index}`">
                <span class="sr-only">
                    Bond {{ index + 1 }} Quality 3
                </span>
            </label>
        </div>

        <button :disabled="canRemove" type="button" @click="() => removeBond(bondGuid)">Sacrifice Bond</button>
    </div>
</template>

<script setup lang="ts">
defineProps<{
    bondGuid: string,
    removeBond: (bond: string) => void,
    index: number,
}>()

const bondQuality = ref([])
const canRemove = computed(() => bondQuality.value.length > 1);
const oneDisabled = computed(() => bondQuality.value.length >= 2);
const twoDisabled = computed(() => bondQuality.value.length < 1 || bondQuality.value.length >= 3);
const threeDisabled = computed(() => bondQuality.value.length < 2);

</script>

<style scoped>
input[type="text"] {
    flex: 4;
}

input[type=checkbox] {
    visibility: hidden;
}

.bond-quality {
    width: 45px;
    height: 15px;
    background: black;
    margin: 20px 10px;
    position: relative;
    border-radius: 5px;
    transition: all 0.5s ease;
}

.bond-quality:has(input[type="checkbox"]:checked) {
    background: lightgreen;
}

.bond-quality:has(input[type="checkbox"]:disabled) {
    background: black;
}


.bond-quality label {
    display: block;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    transition: all 0.5s ease;
    cursor: pointer;
    position: absolute;
    top: -2px;
    left: -3px;
    background: red;
}

.bond-quality input[type=checkbox]:checked+label {
    background: green;
    left: 27px;
}

.bond-quality input[type=checkbox]:disabled:not(:checked)+label {
    background: grey;
}
</style>
