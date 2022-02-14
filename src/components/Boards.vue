<template>
  <div class="hello">
    <div class="boards" v-if="boardsReady">
      <Board v-for="board in boards" :key="board.id" 
      :board="board" 
      @addList="addList"
      @submit="addItem"
      :isOpen="isOpen">
      </Board>
      <div class="new-board">
        <div class="input wrap">
          <input type="text" v-model="newBoardName" placeholder="board name"/>
        </div>
        <div class="icon-wrap">
          <BootstrapIcon  @click="addBoard()"
          icon="plus-circle"
          size="4x"
          flip-v />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Board from '@/components/Board.vue'
import { uuid } from 'vue-uuid'
import BootstrapIcon from '@dvuckovic/vue3-bootstrap-icons';

export default {
  name: 'Boards',
  data: function () {
    return {
      boards: [],
      boardsReady: false,
      isOpen: false,
      newBoardName: ''
    }
  },
  methods: {
    // tu by mala byt este metoda ktora bude validovat ci som vyplnil meno alebo v emite podla toho kde chcem chybovu hlasku
    addBoard() {
      function Board(id, name, lists) {
        this.id = id;
        this.name = name;
        this.lists = lists;
      }
      const newID = uuid.v1();
      this.boards.push(new Board(newID, this.newBoardName, [])); 
    },
    addList(id, name) {
      function List(id, name, items) {
        this.id = id;
        this.name = name;
        this.items = items;
      }
      const newID = uuid.v1();      
      for (let i in this.boards) {
        console.log(i);
        if(this.boards[i].id === id) {
          //this.boards[board].push(new List(newID, "Doe", []));
          if(!("lists" in this.boards[i])) {
            this.boards[i].lists = [];
          }
          this.boards[i].lists.push(new List(newID, name, []));
        }
      } 
    },
    addItem(idBoard, idList, name) {    
      function Item(id, name) {
        this.id = id;
        this.name = name;
      }
      const newID = uuid.v1();
      for (let i in this.boards) {
        if(this.boards[i].id === idBoard) {         
          for (let u in this.boards[i].lists) {
            if(this.boards[i].lists[u].id === idList) {
                this.boards[i].lists[u].items.push(new Item(newID, name, []));
            }
          }
        }
      }  
    }
  },
  computed: {
    BoardsReady: function () {
      if (this.boards.length > 0) {
        return true;
      } 
      return false;
    }
  },
  created() {
    // toto by som dal do nejakeho sepratneho subor resp vsetky volania API alebo by som pouzil Axios
    async function fetchMoviesJSON() {
      const response = await fetch('http://localhost:8080/data.json');
      const boards = await response.json();
      return boards;
    }
    fetchMoviesJSON().then(boards => {
      boards;
      this.boards = boards.boards;
      this.boardsReady = true;
    });
  },
  components: {
    Board,
    BootstrapIcon
  }
}
</script>

<!-- farby velkosti fontov by som dal do variables.less a aj less funkciu na prepocet pixelov do remov -->
<style lang="less" scoped>
  .boards {
    display: flex;
    flex-wrap: wrap;
    .new-board {
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 300px;
      min-height: 150px;
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
