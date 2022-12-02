<template>
  <button :style="{ backgroundColor: color }" class="btn">{{ text }}</button>
</template>

<script>
export default {
  name: 'Button',
  props: ['color', 'text'],
};
</script>
