<template>
  <div class="modal" v-if="isVisible">
    <div class="modal-content">
      <slot></slot>
      <button class="close-button" @click="close">Закрыть</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["isVisible"],
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.close-button {
  margin-top: 20px;
  padding: 8px 16px;
  font-size: 14px;
  font-weight: 600;
  color: white;
  border-radius: 4px;
  border: 1px solid #ccc;
  background-color: #ff00009a;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.close-button:hover {
  background-color: #ff0000e1;
}
</style>
