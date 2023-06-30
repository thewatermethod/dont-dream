<template>
    <form id="character-sheet" :class="`sheet ${showDetails ? 'show-details' : ''}`">
        <div>
            <div v-if="showFileUpload">
                <label for="import-file">Upload</label>
                <input type="file" id="import-file" name="import-file" @change="importCharacter" />
            </div>
            <button type="button" @click="openFileUploader">Import</button>
            <button type="button" @click="exportCharacter">Export</button>
        </div>
        <div id="show-details">
            <input type="checkbox" id="details" name="details" v-model="showDetails" />
            <label for="details">Show details</label>
        </div>
        <div class="form-group" id="character-name">
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
            <div v-for="attribute in attributes" class="attribute">
                <label :for="attribute.htmlId">
                    {{ attribute.label }}
                </label>
                <input min="-3" max="3" type="number" :id="attribute.htmlId" :name="attribute.htmlId"
                    v-model="attribute.model" />
            </div>
        </div>

        <div class="span-2 ability-explainer">
            <h2 v-if="showDetails">What do these mean?</h2>
            <p class="explainer">An investigator rolls with BODY if the action involves a contest of strength, a feat of
                athleticism, or a particularly dextrous or physically challenging maneuver.</p>
            <p class="explainer">An investigator rolls with COMPOSURE if they are trying to do something that requires
                intense concentration, a steady hand, or nerves of steel.</p>
            <p class="explainer">An investigator rolls with LIBRARY USE if they are looking to do research in a book or
                computer, analyze some evidence, or turn up an unlikely fact.</p>
            <p class="explainer">An investigator rolls AFFECT if they are trying to charm, intimidate, or entertain.</p>
            <p class="explainer">An investigator rolls POWER if the action involves using or understanding supernatural
                forces or the occult. A keeper can always override the use of any other ability to call for a power roll
                instead.</p>
        </div>

        <div class="form-group" id="bonds">
            <h2>Bonds</h2>
            <p class="explainer">Investigators form bonds with people, places, and other investigators. Bonds have
                between 1 and 3 quality
                points, depending on how powerful the connection is.</p>
            <div class="bond" v-for="(bond, index) in bonds" :key="index">
                <label class="sr-only" :for="`bond_${index}`">Bond</label>
                <input type="text" :name="`bond_${index}`" v-model="bond.name" :id="`bond_${index}`">
                <label class="sr-only" :for="`bond_qp_${index}`">Quality points</label>
                <input type="number" :name="`bond_qp_${index}`" v-model="bond.qp" :id="`bond_qp_${index}`" min="0" max="3">
                <button type="button" @click="() => removeBond(index)">[x]</button>
            </div>
            <button @click="appendBond" type="button">+ Bond</button>
        </div>

        <div class="form-group" id="vice-group">
            <h2><label for="vice">Vice</label></h2>
            <p class="explainer">Investigators have a vice they can indulge during downtime in order to regain points in its
                governing vice.</p>
            <input type="text" id="vice" name="vice" v-model="vice" />
            <label for="vice_attribute">Vice attribute</label>
            <select id="vice_attribute" name="vice_attribute" v-model="viceAttribute">
                <option>Health</option>
                <option>Reality</option>
            </select>
        </div>

        <div class="form-group" id="playbook-group">
            <h2>Playbook</h2>
            <label for="playbook">Choose a playbook</label>
            <select id="playbook" name="playbook" v-model="playbook" @change="resetMoves">
                <option v-for="option in pbOptions" :key="option._id" :value="option._path">
                    {{ option.title }}
                </option>
            </select>
        </div>

        <ContentQuery v-if="showDetails" v-slot="{ data }" :path="playbook">
            <div class="explainer span-4" v-for="doc in data" :key="doc._id">
                <ContentRenderer :value="doc" />
            </div>
        </ContentQuery>


        <ContentQuery v-if="playbook" :path="`/moves${playbook.replace('/playbooks', '')}`" v-slot="{ data }">
            <h2 class="span-4 no-margin">Moves (choose 2)</h2>
            <div class="form-group span-4 form-group--playbook__move" v-for="doc in data" :key="doc._id">
                <h3 class="no-margin">{{ doc.title }}</h3>
                <ContentRenderer v-if="showDetails" :value="doc" />
                <input :id="doc._dir" :value="doc.title" v-model="selectedMoves" type="checkbox" />
            </div>
        </ContentQuery>

    </form>
</template>

<script setup lang="ts">

const name = ref('');
const body = ref(0);
const power = ref(0);
const composure = ref(0);
const libraryUse = ref(0);
const affect = ref(0);
const selectedMoves = ref([]);

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

const vice = ref('');
const viceAttribute = ref('');
const health = ref(3);
const reality = ref(3);
const playbook = ref('');
const showDetails = ref(true);
const bonds = ref([{
    name: '',
    qp: 0
}])

const showFileUpload = ref(false);

function openFileUploader() {
    showFileUpload.value = true;
}


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
            vice: vice.value,
            viceAttribute: viceAttribute.value,
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
    reader.readAsText(file);


}

const pbOptions = (await queryContent('/playbooks').find()).filter((p) => p._path !== '/playbooks');



</script>

<style>
:root {
    --input-max-width: 350px;
    --input-padding: 0.5rem;
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

.sheet {
    background: white;
    display: grid;
    gap: 1rem;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    padding: 1.5rem;
    position: relative;
}

.sheet h2 {
    font-family: 'VallejoSerifBlack', cursive;
}

#show-details,
#playbook-group,
.span-4 {
    grid-column: 1 / span 4;
}

#show-details {
    background: white;
    display: flex;
    justify-content: center;
    gap: 0.5rem;
    position: sticky;
    top: 87px;
}

#character-name,
#bonds {
    grid-column: 1 / span 2;
}

.span-2 {
    grid-column: auto / span 2;
}

#vice-group {
    grid-column: 3 / span 2;
}

#attributes {
    display: flex;
    align-items: center;
    min-height: 500px;
}

.attribute {
    display: flex;
    width: clamp(320px, 50%, 600px);
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
    display: flex;
    gap: 0.5rem;
}

.bond input[type="text"] {
    flex: 2;
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