<template>
  <div class="modal is-active">
    <div class="modal-background"></div>
    <div class="modal-content">
      <div class="card">
        <div class="card-header">
          <div class="card-header-title">Add Board</div>
        </div>
        <div class="card-content">
          <div
            class="notification p-3 mb-2 is-danger is-light"
            v-if="name_error"
          >
            {{ name_error }}
          </div>
          <div
            class="notification p-3 mb-2 is-danger is-light"
            v-if="lists_error"
          >
            {{ lists_error }}
          </div>
          <label>Board Name</label>
          <div class="control mb-2">
            <input
              class="input"
              type="text"
              v-model="name"
              placeholder="Enter Board Name"
            />
          </div>
          <label>Lists</label>
          <div
            class="list-item p-1 px-3 mb-2"
            v-for="(list, i) in lists"
            :key="i"
          >
            <div>{{ list }}</div>
            <a @click="removeList" class="delete">&times;</a>
          </div>
          <div class="control mb-2">
            <input
              class="input"
              type="text"
              placeholder="Enter List Name"
              v-model="list"
            />
          </div>
          <div class="is-flex is-justify-content-right">
            <button type="button" class="button is-small" @click="addList">
              Add more list
            </button>
          </div>
        </div>
        <footer class="card-footer">
          <a href="#" class="card-footer-item" @click="close">Cancel</a>
          <a href="#" class="card-footer-item" @click="submit">Save</a>
        </footer>
      </div>
    </div>
    <button
      class="modal-close is-large"
      aria-label="close"
      @click="close"
    ></button>
  </div>
</template>

<script>
export default {
  name: "AddBoard",
  props: {
    save: Function,
    close: Function,
  },
  data() {
    return {
      lists: [],
      lists_error: "",
      name_error: "",
      list: "",
      name: "",
    };
  },
  methods: {
    addList() {
      if (!this.list) return;
      this.lists.push(this.list);
      this.list = "";
    },
    removeList(index) {
      this.lists.splice(index, 1);
    },
    submit() {
      if (!this.name) {
        this.name_error = "Please enter a name";
      } else {
        this.name_error = "";
      }
      if (!this.lists.length && !this.list) {
        this.lists_error = "Please add at least one list ";
      } else {
        this.lists_error = "";
      }

      if (this.lists_error || this.name_error) return;

      this.save({
        name: this.name,
        lists: [...this.lists, this.list]
          .filter((name) => name)
          .map((name) => ({
            name,
            items: [],
          })),
      });
      this.close();
    },
  },
};
</script>

<style>
.list-item {
  display: flex !important;
  width: 100%;
  justify-content: space-between !important;
  border: solid 1px #dddddf;
  border-radius: 6px;
}
</style>
