<template>
  <div class="item">
    {{item.name}}
  </div>
</template>

<script>
export default {
  name: 'List',
  props: {
    item: Object
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.item {
  background-color: #fbb120;
  padding: 15px;
  margin: 15px;
  border-radius: 10px;
}
</style>
