<script setup lang="ts">
const props = defineProps({
  class: {
    type: String,
  },
  layoutClass: {
    type: String,
  },
});
</script>

<template>
  <div class="slidev-layout section" :class="$props.layoutClass">
    <div :class="$props.class">
      <slot />
      <img src="../assets/waves-2.svg" alt="NWS waves" />
    </div>
  </div>
</template>
