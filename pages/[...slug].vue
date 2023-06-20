<template>
  <div class="content">
    <ContentDoc v-slot="{ doc }">
      <h1>{{ doc.title }}</h1>
      <ContentRenderer :value="doc" />

      <div class="rules-nav">
        <ContentQuery v-if="doc.prev" :path="`/`" :where="{ rules_page: doc.prev }" v-slot="{ data }">
          <RulesNav v-for="page in data" :key="page.id" :doc="page" :className="`rules-prev`" />
        </ContentQuery>

        <ContentQuery v-if="doc.next" :path="`/`" :where="{ rules_page: doc.next }" v-slot="{ data }">
          <RulesNav v-for="page in data" :key="page.id" :doc="page" :className="`rules-next`" />
        </ContentQuery>

        <NuxtLink v-if="!doc.next" to="/playbooks" className="rules-next">Playbooks</NuxtLink>
      </div>
    </ContentDoc>
  </div>
</template>

<style>
.content h2 a,
.content h3 a {
  color: inherit;
}


.rules-nav {
  border-top: 1px solid;
  display: flex;
  justify-content: space-between;
}

.rules-nav a {
  align-items: center;
  display: flex;
  height: 60px;
  justify-content: center;
  position: relative;
}

.rules-nav:not(:has(.rules-prev)) {
  flex-direction: column;
}

.rules-nav:not(:has(.rules-prev)) a:last-of-type {
  align-self: flex-end;
}

.rules-prev:before,
.rules-next:after {
  background: url('/assets/arrow.svg') 100% no-repeat;
  background-size: cover;
  content: '';
  height: 3rem;
  width: 3rem;
}

.rules-prev:before {
  margin-right: 1rem;
}

.rules-next:after {
  margin-left: 1rem;
  transform: rotate(180deg);
}
</style>