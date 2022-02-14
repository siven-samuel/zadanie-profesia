<template>
  <!-- totu by som dal nejaky <transtion> aby ta animacia otvorenia komponetu bola plynulejsia -->
  <div class="board" @click.self="isOpen=true" :class="isOpen ? 'open' : 'closed'">
    <div class="board-name">{{board.name}}</div>
    <div v-if="isOpen" class="close" @click="isOpen=false">
        <BootstrapIcon 
        icon="backspace-fill"
        size="2x"
        flip-v />
    </div>

    <div class="lists" @click.self="isOpen=true">
        <list v-for="list in board.lists" :key="list.id" 
        :isOpen="isOpen"
        :boardId="board.id"
        :list="list">
        </List>
        <div v-if="isOpen" class="new-list">
            <div class="input wrap">
                <input type="text" v-model="newListName" placeholder="list name"/>
            </div>
            <div class="icon-wrap">
                <BootstrapIcon @click="$emit('addList', board.id, newListName)"
                icon="plus-circle"
                size="4x"
                flip-v />
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import List from '@/components/List.vue'
import BootstrapIcon from '@dvuckovic/vue3-bootstrap-icons';

export default {
  name: 'Board',
  props: {
    board: Object,
  },
  data: function () {
    return {
      isOpen: false,
      newListName: ''
    }
  },
  methods: {
    showAlert(str) {
      alert(str);
    },
  },
  components: {
    List,
    BootstrapIcon
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>

.board {
    background-color: #efefec;
    margin: 20px;
    width: 300px;
    height: 300px;
    overflow: auto;
    border-radius: 15px;
    transition: 1s all;
    cursor: pointer;
    &:hover {
        opacity: 0.6;
    }
    .close {
        position: absolute;
        right: 15px;
        cursor: pointer;
        top: 10px;
    }
    .board-name {
        padding: 15px;
        background-color: #fbb120;
        color: white;
        pointer-events: none;
        min-height: 54px;
    }
    .lists {
        overflow: auto;
        height: calc(100% - 54px);
    }
    &.open {
        &:hover {
            opacity: 1;
        }
        position: fixed;
        cursor: inherit;
        z-index: 1;
        top: 0;
        left: 0;
        margin: 0;
        width: 100%;
        height: 100vh;
        overflow: auto;
        border-radius: 0;
        .lists {
            display: flex;
            flex-wrap: wrap;
            pointer-events: all;
        }
    }
    .new-list {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 300px;
      min-height: 150px;
      max-height: 300px;
      background-color: #efefec;
      margin: 15px;
      border-radius: 15px;
      box-shadow: 5px 5px 15px 5px rgba(0,0,0,0.24);
      transition: box-shadow 1s;
      cursor: pointer;
      &:hover {
        box-shadow: 2px 2px 5px 2px rgba(0,0,0,0.24);
      }
      .wrap {
        position: relative;
        width: 100%;
        input {
          width: 100%;
          padding: 15px;
          text-align: center;
          border: 0;
          border-radius: 15px 15px 0 0;
        }
        &::before {
          content: "";
          display: block;
          height: 1px;
          background: #bdbdb1;
          width: calc(100% - 20px);
          position: absolute;
          bottom: 12px;
          left: 7.5px;
        }
      }
      .icon-wrap {
          width: 100%;
          height: 100%;
          display: flex;
          justify-content: center;
          align-items: center;
      }
    }
}
</style>
