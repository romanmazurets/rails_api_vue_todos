<template>
  <div class="todos">
    {{ message }}
  </div>
</template>

<script>
export default {
  name: 'Todos',
  data() {
    return { message: 'I am data!' };
  },
};
</script>

<style>
  .todos {
    color: #333;
    background: peachpuff;
  }
</style>
