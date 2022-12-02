<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homePage"
      :color="!showForm ? 'green' : 'red'"
      :text="showForm ? 'Close' : 'Add Task'"
      @click="$emit('toggle-form')"
    />
  </header>
</template>

<script>
import Button from './button.vue';

export default {
  name: 'Header',
  props: {
    title: String,
    showForm: Boolean,
  },
  components: {
    Button,
  },
  computed: {
    homePage() {
      return this.$route.path === '/';
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
