<template>
    <form id="character-sheet" class="sheet">
        <div class="form-group" id="character-name">
            <label for="name">Character name</label>
            <input type="text" name="name" id="name">
        </div>

        <div class="form-group">
            <label for="health">Health</label>
            <input type="number" name="health" id="health" min="0" max="3" v-model="health" />
        </div>

        <div class="form-group">
            <label for="reality">Reality</label>
            <input type="number" name="reality" id="reality" min="0" v-model="sanity">
        </div>

        <div class="form-group" id="bonds">
            <h2>Bonds</h2>
            <div class="bond" v-for="index in 10" :key="index">
                <label class="sr-only" :for="`bond_${index}`">Bond</label>
                <input type="text" :name="`bond_${index}`" :id="`bond_${index}`">
                <label class="sr-only" :for="`bond_qp_${index}`">Quality points</label>
                <input type="number" :name="`bond_qp_${index}`" :id="`bond_qp_${index}`" min="0" max="3">
            </div>
        </div>

        <div class="form-group" id="vice">
            <label for="vice">Vice</label>
            <input type="text" id="vice" name="vice" />
            <label for="vice_attribute">Vice attribute</label>
            <select id="vice_attribute" name="vice_attribute">
                <option>Health</option>
                <option>Reality</option>
            </select>

        </div>

    </form>
</template>

<script setup lang="ts">

const health = ref(3);
const sanity = ref(3);
const playbook = ref('');


</script>

<style>
:root {
    --input-max-width: 350px;
    --input-padding: 0.5rem;
}

.sheet {
    background: white;
    display: grid;
    gap: 1rem;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    padding: 1.5rem;
}

#character-name,
#bonds {
    grid-column: 1 / span 2;
}

#vice {
    grid-column: 3 / span 2;
}

.form-group {
    background: white;
    color: #333;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    padding: 0.5rem
}

.form-group__row {
    flex-direction: row;
}

.form-group h2 {
    font-size: 1.25rem;
    font-weight: normal;
    margin: 0;
}

/**
    Text inputs
*/
.form-group input[type="text"],
.form-group input[type="number"] {
    border: 1px solid tomato;
    border-radius: 5px;
    padding: var(--input-padding);
    outline: none;
}

.form-group input[type="text"]:focus,
.form-group input[type="text"]:active,
.form-group input[type="number"]:focus,
.form-group input[type="number"]:active {
    border-bottom: 3px solid darkblue;
}

/**
    Text inputs
*/
.form-group select {
    border: 0;
    border: 2px solid tomato;
    outline: none;
    padding: var(--input-padding);
}

.form-group select:focus,
.form-group select:active {
    border-bottom: 3px solid darkblue;
}

/** 
  Bonds 
 */

.bond {
    display: flex;
    gap: 0.5rem;
}

.bond input[type="text"] {
    flex: 2;
}
</style>