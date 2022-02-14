<template>
  <div class="list" :class="isOpen ? 'open' : 'closed'">
    <div class="list-name">{{list.name}}</div>
    <div v-if="isOpen" class="item-list-wrap">
      <div class="add-list">
        <div class="input-group mb-3">
          <input type="text" class="form-control" placeholder="Item name" v-model="newItemName">
          <div class="input-group-append">
              <button @click="addItem" class="btn btn-outline-secondary" type="button">
                <BootstrapIcon 
                icon="plus-circle"
                flip-v />
              </button>
          </div>
        </div>
      </div>
      <div class="item-list">
        <item v-for="item in list.items" :key="item.id" :isOpen="isOpen" :item="item"></item>
      </div>
    </div>
  </div>
</template>

<script>
import Item from '@/components/Item.vue'
import BootstrapIcon from '@dvuckovic/vue3-bootstrap-icons';

export default {
  name: 'List',
  props: {
    list: Object,
    isOpen: Boolean,
    boardId: String,
  },
  data: function () {
    return {
      newItemName: '',
    }
  },
  methods: {
    addItem(){
      // console.log(this.boardId);
      this.$parent.$emit('submit', this.boardId, this.list.id, this.newItemName)
    }
  },
  components: {
    Item,
    BootstrapIcon
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.list {
    background-color: #ed2a31;
    margin: 15px;
    padding: 15px;
    color: white;
    border-radius: 5px;
    position: relative;
    .item-list {
      display: flex;
      flex-direction: column-reverse;
    }
    &.open {
      width: 300px;
      height: 300px;
      overflow: hidden;
      background-color: #fff;
      color: #000;
      padding: 0;
      .item-list-wrap {
        overflow: auto;
        height: calc(100% - 54px);
      }
      .list-name {
        background-color: #ed2a31;
        color: #fff;
        padding: 15px;
        min-height: 54px;
      }
    }
    .input-group {
      padding: 15px 15px 0 15px;
    }
}
</style>
