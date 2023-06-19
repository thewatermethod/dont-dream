<template>
    <div class="content">
        <ContentDoc v-slot="{ doc }">
            <h1>{{ doc.title }}</h1>
            <ContentRenderer :value="doc" />
            <ContentNav path="/playbooks" />
        </ContentDoc>
    </div>
</template>

<script setup lang="ts">



</script>
  
