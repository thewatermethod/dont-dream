<template>
    <form id="character-sheet" class="sheet">
        <SheetNav />

        <div class="form-group">
            <label for="playbook">Playbook</label>
            <select name="playbook" v-model="playbook">
                <ContentQuery path="/playbooks" v-slot="{ data }">
                    <option v-for="pb in data" :key="pb.id">
                        {{ pb.title }}
                    </option>
                </ContentQuery>
            </select>
        </div>

        <div v-if="playbook" class="form-group-section">
            <ContentQuery :path="`/moves`" :where="{ playbook }" v-slot="{ data }">
                <Move v-for="move in data" :move="move" :key="move.id" />
            </ContentQuery>
        </div>
    </form>
</template>


<script setup lang="ts">
const playbook = ref('');
</script>
