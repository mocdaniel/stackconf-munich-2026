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
  <div class="slidev-layout cover" :class="$props.layoutClass">
    <div :class="$props.class">
      <slot />
      <img src="../assets/waves-1.svg" alt="NWS waves" />
    </div>
  </div>
</template>
