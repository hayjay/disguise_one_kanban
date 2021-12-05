<template>
  <div :class="{ 'is-active': show, modal: true }">
    <div class="modal-background"></div>
    <div class="modal-content">
      <div class="card">
        <div class="card-header">
          <div class="card-header-title">Confirm</div>
        </div>
        <div class="card-content">
          <div
          >
            {{ message }}
          </div>
          </div>
          <footer class="card-footer">
            <a href="#" class="card-footer-item" @click="_cancel">Cancel</a>
            <a href="#" class="card-footer-item" @click="_confirm">Proceed</a>
          </footer>
        
      </div>
      <button
        class="modal-close is-large"
        aria-label="close"
        @click="_cancel"
      ></button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ConfirmModal",
  data() {
    return {
      confirm: () => null,
      message: "",
      show: false,
    };
  },
  methods: {
    open(opt = { confirm: () => null }) {
      this.message = opt.message;
      this.confirm = opt.confirm;
      this.show = true;
    },

    _confirm() {
      this.confirm();
      this.confirm = () => null;
      this.show = false;
    },

    _cancel() {
      this.confirm = () => null;
      this.show = false;
    },
  },
};
</script>

<style></style>
