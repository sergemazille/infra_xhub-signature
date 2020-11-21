<template>
  <div id="app">
    <InputForm />
  </div>
</template>

<script>
import InputForm from './components/InputForm.vue';

export default {
  name: 'App',
  components: {
    InputForm,
  },
};
</script>

<style lang="scss" scoped>
#app {
  margin: 20px;
}
</style>
