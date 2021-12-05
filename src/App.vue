<template>
  <div class="has-background-primary-light app-wrapper">
    <Header />
    <div class="p-5">
      <div class="navigation is-flex is-justify-content-space-between">
        <div>
          <div class="select is-success mr-2">
            <select v-model="selectedBoardIndex">
              <option
                v-for="(board, i) in boards"
                :key="i + '--index--' + board.name"
                :value="i"
              >
                {{ board.name }}
              </option>
            </select>
          </div>
          <button
            class="button is-primary mr-2 is-outlined"
            @click="toggleShowAddBoardModal"
          >
            Add Board
          </button>
          <button
            class="button is-primary mr-2 is-outlined"
            @click="toggleShowAddListModal"
          >
            Add List
          </button>
          <button class="button is-primary" @click="toggleShowAddTaskModal">
            Add Task
          </button>
        </div>
        <div>
          <button class="button is-danger" @click="removeBoard">
            Remove Board
          </button>
        </div>
      </div>
      <div class="">
        <Board
          :board="boards[selectedBoardIndex]"
          @moveItem="moveItem"
          @moveList="moveList"
          v-if="boards.length"
          :removeTask="removeTask"
          :removeList="removeList"
        />
        <div class="box is-warning is-light has-text-centered" v-else>
          <div class="mb-2 is-size-5">You do not have a board</div>
          <button class="button is-primary" @click="toggleShowAddBoardModal">
            Add New
          </button>
        </div>

        <AddBoard
          v-if="showAddBoardModal"
          :close="toggleShowAddBoardModal"
          :save="addBoard"
        />
        <AddList
          v-if="showAddListModal"
          :close="toggleShowAddListModal"
          :save="addList"
        />
        <AddTask
          v-if="showAddTaskModal"
          :close="toggleShowAddTaskModal"
          :save="addTask"
          :lists="this.boards[this.selectedBoardIndex].lists"
        />
        <ConfirmModal ref="confirm" />
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Board from "./components/Board.vue";
import AddBoard from "./components/AddBoard.vue";
import AddList from "./components/AddList.vue";
import AddTask from "./components/AddTask.vue";
import ConfirmModal from "./components/ConfirmModal.vue";

const boards = [
  {
    name: "Project Sun",
    lists: [
      {
        name: "Todo",
        items: [
          { title: "todo 1", description: "lorem ipsum something happened" },
          { title: "todo 2", description: "lorem ipsum something happened" },
          { title: "todo 3", description: "lorem ipsum something happened" },
        ],
      },
      {
        name: "Doing",
        items: [
          { title: "doings 1", description: "lorem ipsum something happened" },
          { title: "doings 2", description: "lorem ipsum something happened" },
          { title: "doings 3", description: "lorem ipsum something happened" },
          { title: "doings 4", description: "lorem ipsum something happened" },
          { title: "doings 5", description: "lorem ipsum something happened" },
        ],
      },
      {
        name: "Done",
        items: [
          { title: "done 1", description: "lorem ipsum something happened" },
          { title: "done 2", description: "lorem ipsum something happened" },
        ],
      },
    ],
  },
  {
    name: "Project Number",
    lists: [
      {
        name: "Backlog",
        items: [
          {
            title: "backlogs 1",
            description: "lorem ipsum something happened",
          },
          {
            title: "backlogs 2",
            description: "lorem ipsum something happened",
          },
          {
            title: "backlogs 3",
            description: "lorem ipsum something happened",
          },
        ],
      },
      {
        name: "Todo",
        items: [
          { title: "todo 1", description: "lorem ipsum something happened" },
          { title: "todo 2", description: "lorem ipsum something happened" },
          { title: "todo 3", description: "lorem ipsum something happened" },
        ],
      },
      {
        name: "In-progress",
        items: [
          {
            title: "in-progress 1",
            description: "lorem ipsum something happened",
          },
          {
            title: "in-progress 2",
            description: "lorem ipsum something happened",
          },
          {
            title: "in-progress 3",
            description: "lorem ipsum something happened",
          },
          {
            title: "in-progress 4",
            description: "lorem ipsum something happened",
          },
          {
            title: "in-progress 5",
            description: "lorem ipsum something happened",
          },
        ],
      },
      {
        name: "Done",
        items: [
          { title: "done 1", description: "lorem ipsum something happened" },
          { title: "done 2", description: "lorem ipsum something happened" },
        ],
      },
    ],
  },
];

export default {
  name: "App",
  components: {
    Header,
    Board,
    AddBoard,
    AddList,
    AddTask,
    ConfirmModal,
  },
  data() {
    return {
      boards: [],
      selectedBoardIndex: 0,
      showAddBoardModal: false,
      showAddListModal: false,
      showAddTaskModal: false,
    };
  },
  mounted() {
    this.loadBoard();
  },
  methods: {
    moveItem(event, listIndex) {
      const { removedIndex, addedIndex, payload } = event;
      if (removedIndex !== null) {
        this.boards[this.selectedBoardIndex].lists[listIndex].items.splice(
          removedIndex,
          1
        );
      }

      if (addedIndex !== null) {
        this.boards[this.selectedBoardIndex].lists[listIndex].items.splice(
          addedIndex,
          0,
          payload
        );
      }

      this.updateBoard();
    },

    moveList(event) {
      const { removedIndex, addedIndex, payload } = event;
      if (removedIndex !== null) {
        this.boards[this.selectedBoardIndex].lists.splice(removedIndex, 1);
      }

      if (addedIndex !== null) {
        this.boards[this.selectedBoardIndex].lists.splice(
          addedIndex,
          0,
          payload
        );
      }

      this.updateBoard();
    },

    updateBoard() {
      localStorage.setItem("boards", JSON.stringify(this.boards));
    },

    loadBoard() {
      const oldState = localStorage.getItem("boards");
      if (oldState) {
        return (this.boards = JSON.parse(oldState));
      }
      return (this.boards = boards);
    },

    addBoard(board) {
      this.boards.push(board);
      this.selectedBoardIndex = this.boards.length - 1;
      this.updateBoard();
    },

    addList(list) {
      this.boards[this.selectedBoardIndex].lists.push(list);
      this.updateBoard();
    },

    addTask(task, list) {
      this.boards[this.selectedBoardIndex].lists[list].items.push(task);
      this.updateBoard();
    },

    removeBoard() {
      this.$refs.confirm.open({
        message: "Are sure you want to remove this board",
        confirm: () => {
          this.boards.splice(this.selectedBoardIndex, 1);
          this.selectedBoardIndex = 0;
          this.updateBoard();
        },
      });
    },

    removeList(index) {
      console.log(index);
      this.$refs.confirm.open({
        message: "Are sure you want to remove this task list",
        confirm: () => {
          this.boards[this.selectedBoardIndex].lists.splice(index, 1);
          this.updateBoard();
        },
      });
    },

    removeTask(list, index) {
      this.$refs.confirm.open({
        message: "Are sure you want to remove this task",
        confirm: () => {
          this.boards[this.selectedBoardIndex].lists[list].items.splice(
            index,
            1
          );
          this.updateBoard();
        },
      });
    },

    toggleShowAddBoardModal() {
      this.showAddBoardModal = !this.showAddBoardModal;
    },

    toggleShowAddListModal() {
      this.showAddListModal = !this.showAddListModal;
    },

    toggleShowAddTaskModal() {
      this.showAddTaskModal = !this.showAddTaskModal;
    },
  },
};
</script>

<style>
html {
  overflow: auto !important;
}
.app-wrapper {
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}

.app-wrapper ::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

/* Track */
.app-wrapper ::-webkit-scrollbar-track {
  box-shadow: inset 0 0 5px rgb(170, 170, 170);
  background-color: #85eede;
  border-radius: 5px;
}

/* Handle */
.app-wrapper ::-webkit-scrollbar-thumb {
  background: #008a75;
  border-radius: 5px;
}

/* Handle on hover */
.app-wrapper ::-webkit-scrollbar-thumb:hover {
  background: #00574a;
}
</style>
