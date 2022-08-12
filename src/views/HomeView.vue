<template>
  <div class="home">
    <users-tree />
  </div>
</template>

<script>
import UsersTree from '@/components/UsersTree.vue';

export default {
  name: 'HomeView',
  components: {
    'users-tree': UsersTree,
  },
};
</script>
