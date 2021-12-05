<template>
  <div class="board mt-3 has-background-primary p-2">
    <div class="board-list">
      <Container
        group-name="lists"
        @drop="moveList"
        drag-class="card-ghost"
        drop-class="card-ghost-drop"
        :get-child-payload="getListPayload"
        orientation="horizontal"
      >
        <Draggable
          v-for="(list, i) in board.lists"
          :key="i + '--index--' + list.name"
        >
          <TaskList
            :list="list"
            :listIndex="i"
            @moveItem="moveItem"
            :removeTask="removeTask"
            :removeList="removeList"
          />
        </Draggable>
      </Container>
    </div>
  </div>
</template>

<script>
import { Container, Draggable } from "vue-smooth-dnd";
import TaskList from "./TaskList.vue";
export default {
  name: "Board",
  components: { TaskList, Container, Draggable },
  props: {
    board: Object,
    removeList: Function,
    removeTask: Function,
  },
  methods: {
    moveItem(event, index) {
      this.$emit("moveItem", event, index);
    },
    moveList(event) {
      this.$emit("moveList", event);
    },
    getListPayload(index) {
      return this.board?.lists[index];
    },
  },
};
</script>

<style scoped>
.board {
  border-radius: 6px;
}
.board-list {
  overflow-y: auto;
  min-height: calc(100vh - 170px);
}
</style>
