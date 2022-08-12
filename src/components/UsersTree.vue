<template>
  <div class="tree">
    <tree-node v-for="treeNode in treeNodes" :key="treeNode.id" v-bind="treeNode"></tree-node>
  </div>
</template>
<script>
import TreeJSON from '@/store/tree.json';
import TreeNode from './TreeNode.vue';

export default {
  components: { TreeNode },
  data() {
    return {
      treeNodes: TreeJSON.resultDetails,
      treeNode: TreeNode,
    };
  },
  mounted() {
    console.log(this.treeNodes);
  },
};
</script>
<style lang="sass" scoped></style>
