<template>
    <form id="character-sheet" :class="`sheet ${showDetails ? 'show-details' : ''}`">
        <div v-if="showFileUpload">
            <div>
                <label for="import-file">Upload</label>
                <input type="file" id="import-file" name="import-file" @change="importCharacter" />
            </div>
            <button type="button" :disabled="!readyToImport" @click="completeImport">Import</button>
            <button type="button" @click="exportCharacter">Export</button>
        </div>

        <div class="form-group span-2" id="character-name">
            <label for="name">Character name</label>
            <input type="text" name="name" id="name" v-model="name">
        </div>

        <div class="form-group">
            <label for="health">Health</label>
            <input type="number" name="health" id="health" min="0" max="3" v-model="health" />
        </div>

        <div class="form-group">
            <label for="reality">Reality</label>
            <input type="number" name="reality" id="reality" min="0" v-model="reality">
        </div>

        <div class="form-group span-2" id="attributes">
            <h2>Attributes</h2>
            <p class="explainer">To start, choose one ability score to be at 2, one ability score to be at 1 and another to
                be -1. The
                others start at
                +0.</p>
            <div v-for="attribute in attributes" class="attribute">
                <label :for="attribute.htmlId">
                    {{ attribute.label }}
                </label>
                <input min="-3" max="3" type="number" :id="attribute.htmlId" :name="attribute.htmlId"
                    v-model="attribute.model" />
            </div>
        </div>


        <div class="span-2 callout-box">
            <h2>What do these mean?</h2>
            <p>An investigator rolls with BODY if the action involves a contest of strength, a feat of
                athleticism, or a particularly dextrous or physically challenging maneuver.</p>
            <p>An investigator rolls with COMPOSURE if they are trying to do something that requires
                intense concentration, a steady hand, or nerves of steel.</p>
            <p>An investigator rolls with LIBRARY USE if they are looking to do research in a book or
                computer, analyze some evidence, or turn up an unlikely fact.</p>
            <p>An investigator rolls AFFECT if they are trying to charm, intimidate, or entertain.</p>
            <p>An investigator rolls POWER if the action involves using or understanding supernatural
                forces or the occult. A keeper can always override the use of any other ability to call for a power roll
                instead.</p>
        </div>

        <div class="form-group span-2" id="bonds">
            <h2>Bonds</h2>
            <p class="explainer">Investigators form bonds with people, places, and other investigators. Bonds have
                between 1 and 3 quality
                points, depending on how powerful the connection is.</p>
            <p class="explainer">You start with three points, divided between one bond to a character in town and one to
                another Player Character.</p>
            <div class="bordered bond" v-for="(bond, index) in bonds" :key="index">
                <div class="form-group">
                    <label :for="`bond_${index}`">Bond</label>
                    <input type="text" :name="`bond_${index}`" v-model="bond.name" :id="`bond_${index}`">
                </div>
                <div class="form-group">
                    <label :for="`bond_qp_${index}`">Quality points</label>
                    <input type="number" :name="`bond_qp_${index}`" v-model="bond.qp" :id="`bond_qp_${index}`" min="0"
                        max="3">
                </div>
                <button type="button" @click="() => removeBond(index)">[x]</button>
            </div>
            <button @click="appendBond" type="button">+ Bond</button>
        </div>

        <div class="form-group span-4" id="playbook-group">
            <h2>Playbook</h2>
            <label for="playbook">Choose a playbook</label>
            <select id="playbook" name="playbook" v-model="playbook" @change="resetMoves">
                <option v-for="option in pbOptions" :key="option._id" :value="option._path">
                    {{ option.title }}
                </option>
            </select>
        </div>


        <div class="explainer span-4" v-if="playbookDetails">
            <ContentRenderer :value="playbookDetails" />
        </div>


        <ContentQuery v-if="playbook" :path="`/moves${playbook.replace('/playbooks', '')}`" v-slot="{ data }">
            <h2 class="span-4 no-margin">Moves </h2>
            <div class="form-group span-4 form-group--playbook__move" v-for="doc in data" :key="doc._id">
                <h3 class="no-margin">{{ doc.title }}</h3>
                <ContentRenderer :value="doc" />
                <!-- <input :id="doc._dir" :value="doc.title" v-model="selectedMoves" type="checkbox" /> -->
            </div>
        </ContentQuery>

    </form>
</template>

<script setup lang="ts">
useHead({
    title: 'Character Sheet | Don\'t Dream',
})

const name = ref('');
const body = ref(0);
const power = ref(0);
const composure = ref(0);
const libraryUse = ref(0);
const affect = ref(0);
const selectedMoves = ref([]);
const playbookDetails = ref();

const attributes = [
    {
        label: 'Body',
        model: body,
        htmlId: 'body'
    },
    {
        label: 'Power',
        model: power,
        htmlId: 'power'
    },
    {
        label: 'Composure',
        model: composure,
        htmlId: 'composure'
    },
    {
        label: 'Library Use',
        model: libraryUse,
        htmlId: 'library-use'
    },
    {
        label: 'Affect',
        model: affect,
        htmlId: 'affect'
    }
];

const health = ref(3);
const reality = ref(3);
const playbook = ref('');
const showDetails = ref(true);
const bonds = ref([{
    name: '',
    qp: 0
}]);

const showFileUpload = ref(false);
const readyToImport = ref(true);

function appendBond() {
    bonds.value.push({
        name: '',
        qp: 0
    })
}

function removeBond(index: number) {
    bonds.value.splice(index, 1);
}

function resetMoves() {
    selectedMoves.value = [];
}

function exportCharacter() {
    try {
        const sheet = {
            health: health.value,
            reality: reality.value,
            playbook: playbook.value,
            bonds: bonds.value,
            body: body.value,
            power: power.value,
            composure: composure.value,
            libraryUse: libraryUse.value,
            affect: affect.value,
            selectedMoves: selectedMoves.value,
            showDetails: showDetails.value,
            name: name.value,
        }

        const data = JSON.stringify(sheet);
        const blob = new Blob([data], { type: 'application/json' });
        const url = URL.createObjectURL(blob);

        const link = document.createElement('a');
        link.href = url;
        link.download = `${name.value}.dream`;
        link.click();

        URL.revokeObjectURL(url);

    } catch (e) {
        console.error(e);
    }
}

function importCharacter(e: Event) {
    const target = e.target as HTMLInputElement;
    const file = target.files?.[0];
    if (!file) {
        return;
    }

    const reader = new FileReader();
    reader.onload = (evt) => {
        // console.log(evt.target?.result);
        try {
            const char = JSON.parse(evt.target?.result as string);
            console.log(char);
        } catch (e) {
            console.error(e);
        }
    };
}

const pbOptions = (await queryContent('/playbooks').find()).filter((p) => p._path !== '/playbooks');

watchEffect(async () => {
    if (playbook.value) {
        playbookDetails.value = await queryContent(playbook.value).findOne()

    }
});


</script>

<style>
:root {
    --input-max-width: 350px;
    --input-padding: 0.5rem;
}

@media(min-width: 800px) {
    .sheet {
        display: grid;
        gap: 1rem;
        grid-template-columns: repeat(4, 25%);
    }
}

.sheet {
    background: white;
    padding: 1.5rem;
    position: relative;
}

/** grid helpers */

.span-2 {
    grid-column: auto / span 2;
}

.span-3 {
    grid-column: auto / span 3;
}

.span-4 {
    grid-column: 1 / span 4;
}

.sheet h2 {
    font-family: 'VallejoSerifBlack', cursive;
}

/**
    detailed explanations
*/
.explainer {
    display: none;
    font-style: italic;
    margin: 0;
}

.show-details .explainer {
    display: block;
}

.ability-explainer h2 {
    font-size: 1em;
}

.ability-explainer .explainer {
    margin-bottom: 0.5em;
}



#show-details {
    background: white;
    display: flex;
    justify-content: center;
    gap: 0.5rem;
    position: sticky;
    top: 87px;
}



#attributes {
    display: flex;
    align-items: center;
    min-height: 500px;
}

.attribute {
    display: flex;
    margin: 0 auto;
    width: 95%;
}

.attribute label {
    flex: 3;
}

.attribute input {
    flex: 1;
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
    font-size: 1em;
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
    position: relative;
}

.bond button {
    position: absolute;
    top: 0;
    right: 0;
}

/**
playbook moves
 */

.form-group--playbook__move {
    border: 1px solid #333;
    border-radius: 5px;
    padding: 0.5rem;
    margin-bottom: 1rem;
    position: relative;
}

.form-group--playbook__move input[type="checkbox"] {
    position: absolute;
    top: 1em;
    right: 1em;
}

.form-group--playbook__move:has(:checked) {
    background: lightgoldenrodyellow;
}
</style>