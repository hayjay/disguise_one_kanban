<template>
  <div class="list-container box m-2 p-0 has-background-primary-light">
    <div class="list-heading p-3">
      <h5 class="has-text-weight-semibold">{{ list.name }}</h5>
      <button class="delete" @click="removeList(listIndex)"></button>
    </div>
    <div class="list-body p-3">
      <Container
        group-name="item"
        @drop="(e) => dropItem(e, listIndex)"
        drag-class="card-ghost"
        drop-class="card-ghost-drop"
        :get-child-payload="getCardPayload"
      >
        <Draggable
          v-for="(item, i) in list.items"
          :key="i + '--index--' + item.title"
        >
          <TaskItem :data="item" :removeTask="() => removeTask(listIndex, i)" />
        </Draggable>
      </Container>
    </div>
  </div>
</template>

<script>
import { Container, Draggable } from "vue-smooth-dnd";
import TaskItem from "./TaskItem.vue";
export default {
  name: "TaskList",
  components: { TaskItem, Container, Draggable },
  props: {
    list: Object,
    listIndex: Number,
    removeList: Function,
    removeTask: Function,
  },
  methods: {
    dropItem(event, index) {
      this.$emit("moveItem", event, index);
    },
    getCardPayload(index) {
      return this.list?.items[index];
    },
  },
};
</script>

<style>
.list-container {
  width: 350px;
  min-width: 350px;
  position: relative;
}

.list-container .delete {
  position: absolute;
  right: 5px;
  top: 5px;
}

.smooth-dnd-container {
  height: 100%;
}

.smooth-dnd-draggable-wrapper {
  overflow: visible;
}

.list-heading {
  border-bottom: 1px solid #dddddf;
}

.list-body {
  height: calc(100% - 50px);
}
</style>
