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
  <div class="slidev-layout quote" :class="$props.layoutClass">
    <div class="quote-content" :class="$props.class">
      <img src="../assets/quotes-left.svg" alt="Arrows right" />
      <div>
        <slot />
      </div>
      <img src="../assets/quotes-right.svg" alt="Arrows left" />
    </div>
  </div>
</template>
