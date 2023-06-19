<template>
    <form id="character-sheet" class="sheet">
        <SheetNav />
        <div class="form-group-section">
            <div class="form-group form-group__bond-group">
                <Bond v-for="(bond, index) in bonds" :index="index" :key="bond" :bondGuid="bond" :removeBond="removeBond" />
                <button type="button" @click="addBond">Add Bond</button>
            </div>
        </div>
    </form>
</template>

<script setup lang="ts">
import { v4 as uuidv4 } from 'uuid';

const bonds = ref([
    uuidv4(),
    uuidv4()
]);

function removeBond(bond: string) {
    bonds.value = bonds.value.filter(b => b !== bond);
}

function addBond() {
    bonds.value.push(uuidv4());
}

</script>

<style>
.form-group__bond-group {
    flex-direction: column;
    gap: 2rem;
    padding: 1.5rem;
}
</style>