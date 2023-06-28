<template>
    <div class="content">
        <ContentDoc v-slot="{ doc }">
            <h1>{{ doc.title }}</h1>
            <ContentRenderer :value="doc" />
            <h2>Moves</h2>
            <ContentQuery v-if="doc._path !== '/playbooks'" v-slot="{ data }"
                :path="`${doc._path.replace('playbooks', 'moves')}`">
                <div v-for="move in data" class="playbook-move">
                    <h3> {{ move.title }}</h3>
                    <ContentRenderer :value="move" />
                </div>
            </ContentQuery>
            <div v-if="doc._path === '/playbooks/unseen'" class="rules-nav">
                <hr />
                <NuxtLink class="rules-next" to="/adventures">Sample adventures</NuxtLink>
            </div>
        </ContentDoc>
    </div>
</template>
  
<style>
h2 a,
h3 a {
    color: inherit;
}

.content {
    max-width: 540px;
}
</style>