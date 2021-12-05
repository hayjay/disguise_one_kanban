<template>
  <div class="modal is-active">
    <div class="modal-background"></div>
    <div class="modal-content">
      <div class="card">
        <div class="card-header">
          <div class="card-header-title">Add List</div>
        </div>
        <div class="card-content">
          <div
            class="notification p-3 mb-2 is-danger is-light"
            v-if="name_error"
          >
            {{ name_error }}
          </div>
          <label>List name</label>
          <div class="control mb-2">
            <input
              class="input"
              type="text"
              v-model="name"
              placeholder="Enter name"
            />
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
  name: "AddList",
  props: {
    save: Function,
    close: Function,
    lists: Array,
  },
  data() {
    return {
      name_error: "",
      name: "",
    };
  },
  methods: {
    submit() {
      if (!this.name) {
        this.name_error = "Please enter a name";
      } else {
        this.name_error = "";
      }

      if (this.name_error) return;

      this.save({
        name: this.name,
        items: [],
      });
      this.close();
    },
  },
};
</script>

<style></style>
