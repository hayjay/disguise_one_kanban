<template>
  <div class="modal is-active">
    <div class="modal-background"></div>
    <div class="modal-content">
      <div class="card">
        <div class="card-header">
          <div class="card-header-title">Add Task</div>
        </div>
        <div class="card-content">
          <div
            class="notification p-3 mb-2 is-danger is-light"
            v-if="title_error"
          >
            {{ title_error }}
          </div>
          <div
            class="notification p-3 mb-2 is-danger is-light"
            v-if="list_error"
          >
            {{ list_error }}
          </div>
          <label>Task Title</label>
          <div class="control mb-2">
            <input
              class="input"
              type="text"
              v-model="title"
              placeholder="Enter title"
            />
          </div>
          <label>Description</label>
          <div class="control mb-2">
            <textarea
              class="input"
              type="text"
              v-model="description"
              placeholder="Enter description"
            />
          </div>
          <label>List</label>
          <div class="select is-fullwidth">
            <select v-model="list">
              <option
                v-for="(list, i) in lists"
                :key="i + '--index--' + list.name"
                :value="i"
              >
                {{ list.name }}
              </option>
            </select>
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
  name: "AddTask",
  props: {
    save: Function,
    close: Function,
    lists: Array,
  },
  data() {
    return {
      title_error: "",
      list_error: "",
      list: "",
      title: "",
      description: "",
    };
  },
  methods: {
    submit() {
      if (!this.title) {
        this.title_error = "Please enter a title";
      } else {
        this.title_error = "";
      }
      if (!this.list) {
        this.list_error = "Please select a list ";
      } else {
        this.list_error = "";
      }

      if (this.list_error || this.title_error) return;

      this.save(
        {
          title: this.title,
          description: this.description,
        },
        this.list
      );
      this.close();
    },
  },
};
</script>

<style></style>
