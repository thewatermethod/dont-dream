<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Permanent+Marker&display=swap');

body {
  background: #333;
  color: whitesmoke;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  margin: 0;
}

/** 
  sr accessibility class, borrowed from 
  https://gist.github.com/ffoodd/000b59f431e3e64e4ce1a24d5bb36034
 */
.sr-only {
  border: 0 !important;
  clip: rect(1px, 1px, 1px, 1px) !important;
  /* 1 */
  -webkit-clip-path: inset(50%) !important;
  clip-path: inset(50%) !important;
  /* 2 */
  height: 1px !important;
  margin: -1px !important;
  overflow: hidden !important;
  padding: 0 !important;
  position: absolute !important;
  width: 1px !important;
  white-space: nowrap !important;
  /* 3 */
}
</style>