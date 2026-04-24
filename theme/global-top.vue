<template>
  <header
    v-if="$nav.currentLayout !== 'cover'"
    class="absolute top-0 right-0 px-6 py-3"
  >
    <img
      class="w-24"
      src="./assets/logo.svg"
      alt="NETWAYS Managed Services logo"
    />
  </header>
</template>
