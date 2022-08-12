<template>
  <div class="tree-node">
    <div class="content">
      <div @click="collapse" v-if="totalIssuesLength > 0" class="collapser-container">
        <div v-if="!isCollapsed">+</div>
        <div v-else>-</div>
      </div>
      <div class="name" :class="{ 'extra-indent': totalIssuesLength === 0 }">{{ name }}</div>
      <div class="discovered-issues">{{ discoveredIssues }}</div>
      <div class="total-issues">{{ totalIssuesLength }}</div>
    </div>
    <div v-if="isCollapsed" class="child-nodes-container">
      <tree-node v-for="treeNode in children" :key="treeNode.id" v-bind="treeNode"></tree-node>
    </div>
  </div>
</template>
<script>
export default {
  name: 'TreeNode',
  data() {
    return {
      isCollapsed: false,
    };
  },
  props: {
    name: {
      type: String,
      default: '',
    },
    discoveredIssues: {
      type: Number,
      default: 0,
    },
    children: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  methods: {
    collapse() {
      this.isCollapsed = !this.isCollapsed;
    },
  },
  computed: {
    totalIssuesLength() {
      // we count every inner child's discoverred issues
      // we don't add the discovered issues of itself

      // --- es5 version which i believe is more readable
      // let totalCount = 0;
      // const findChildrenCount = (child) => {
      //   totalCount += child.discoveredIssues;
      //   if (child.children.length) {
      //     child.children.forEach((item) => findChildrenCount(item));
      //   }
      // };
      // this.children.forEach((item) => findChildrenCount(item));
      // return totalCount;

      // --- es6 version
      const findChildrenCount = (children, value) =>
        // eslint-disable-next-line
        children.reduce((acc, curr) => {
          let temp = curr.discoveredIssues + acc;
          if (curr.children.length > 0) {
            temp = findChildrenCount(curr.children, temp);
          }
          return temp;
        }, value);

      return findChildrenCount(this.children, 0);
    },
  },
};
</script>
<style lang="scss" scoped>
.tree-node {
  display: flex;
  flex-direction: column;
  .content {
    display: flex;
    align-items: center;
    .collapser-container {
      cursor: pointer;
    }
    .name {
      margin-left: 8px;
    }
    .discovered-issues {
      background-color: #e5001c;
      padding: 4px;
      margin: 4px;
      color: white;
      border-radius: 8px;
    }
    .total-issues {
      color: #ca0019;
    }
  }
  .child-nodes-container {
    margin-left: 24px;
  }
}
.extra-indent {
  // in order to override an more specific css class,
  // we use important.
  // We could replace this to same hierarchy but
  // we may use it again
  margin-left: 18px !important;
}
</style>
