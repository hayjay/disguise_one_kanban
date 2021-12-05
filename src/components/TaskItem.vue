<template>
  <div class="item-container box mb-2 py-3 px-4">
    <button class="delete" @click="removeTask"></button>
    <h6 class="item-heading is-size-6 has-text-weight-semibold">
      {{ data.title }}
    </h6>
    <div class="item-body is-size-7 has-text-grey">{{ data.description }}</div>
  </div>
</template>

<script>
export default {
  name: "TaskItem",
  props: {
    data: Object,
    removeTask: Function,
  },
};
</script>

<style>
.item-container {
  border: 1px solid #dddddf;
  position: relative;
}

.item-container .delete {
  position: absolute;
  right: 5px;
  top: 5px;
}

.item-heading {
  text-transform: capitalize;
}
</style>
