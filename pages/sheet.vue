<template>
    <form id="character-sheet">
        <div class="form-group">
            <label for="name">Character name</label>
            <input type="text" name="name" id="name">
        </div>



        <div class="form-group">
            <label for="playbook">Playbook</label>
            <select name="playbook" v-model="playbook">
                <ContentQuery path="/playbooks" v-slot="{ data }">
                    <option v-for="pb in data" :key="pb.id" :value="pb.id">
                        {{ pb.title }}
                    </option>
                </ContentQuery>
            </select>
        </div>

        <div class="form-group form-group__multiple">
            <label for="health">Health</label>
            <input type="number" name="health" id="health" min="0" max="3" v-model="health" />

            <label for="sanity">Sanity</label>
            <input type="number" name="sanity" id="sanity" min="0" v-model="sanity">
        </div>

        <Attributes :buildPoints="buildPoints" />

        <div class="form-group-section">
            <h2>Bonds</h2>
            <div class="form-group form-group__bond-group">
                <Bond v-for="(bond, index) in bonds" :index="index" :key="bond" :bondGuid="bond" :removeBond="removeBond" />
                <button type="button" @click="addBond">Add Bond</button>
            </div>
        </div>

        <div v-if="playbook" class="form-group-section">
            <ContentQuery :path="`/moves`" :where="{ playbookId: parseInt(playbook, 10) }" v-slot="{ data }">
                <Move v-for="move in data" :move="move" :key="move.id" />
            </ContentQuery>
        </div>
    </form>
</template>

<script setup lang="ts">
import { v4 as uuidv4 } from 'uuid';

const health = ref(3);
const sanity = ref(3);
const playbook = ref('');
const buildPoints = ref(3);

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

<style scoped>
form {
    margin: 1rem 0;
}

.form-group {
    background: whitesmoke;
    color: #333;
    display: flex;
    gap: 0.5rem;
    padding: 0.5rem;
}

.form-group__bond-group {
    flex-direction: column;
}

/**
    Text inputs
*/
.form-group input[type="text"] {
    border: 0;
    border-bottom: 2px solid lightseagreen;
    outline: none;
}

.form-group input[type="text"]:focus,
.form-group input[type="text"]:active {
    border-bottom: 3px solid darkblue;
}

/**
    Text inputs
*/
.form-group select {
    border: 0;
    border: 2px solid lightseagreen;
    outline: none;
}

.form-group select:focus,
.form-group select:active {
    border-bottom: 3px solid darkblue;
}

@media (min-width: 768px) {

    .form-group select,
    .form-group input[type="text"] {
        min-width: 33vw;
    }
}
</style>