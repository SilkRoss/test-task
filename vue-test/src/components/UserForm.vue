<template>
  <form @submit.prevent="submitForm">
    <div class="form-group">
      <label for="name">Имя:</label>
      <input type="text" id="name" v-model="name" required />
      <span v-if="!name" class="error-message">Имя обязательно</span>
    </div>

    <div class="form-group">
      <label for="phone">Телефон:</label>
      <input type="text" id="phone" v-model="phone" required />
      <span v-if="!isValidPhone" class="error-message"
        >Некорректный формат телефона</span
      >
    </div>

    <div class="form-group">
      <label for="parent">Начальник:</label>
      <select id="parent" v-model="parent">
        <option value="">Без начальника</option>
        <option v-for="user in users" :key="user.id" :value="user.id">
          {{ user.name }}
        </option>
      </select>
    </div>

    <button class="submit-button" type="submit" :disabled="isFormInvalid">
      <span v-if="!isSubmitting">Сохранить</span>
      <span v-else>Сохраняется...</span>
    </button>
  </form>
</template>

<script>
export default {
  props: ["users"],
  data() {
    return {
      name: "",
      phone: "",
      parent: "",
      isSubmitting: false,
    };
  },
  computed: {
    isValidPhone() {
      // Простая проверка формата телефона
      return /^[\d\s()-]+$/.test(this.phone);
    },
    isFormInvalid() {
      return !this.name || !this.isValidPhone || this.isSubmitting;
    },
  },
  methods: {
    submitForm() {
      if (this.isFormInvalid) return;
      this.isSubmitting = true;
      setTimeout(() => {
        this.$emit("submit", {
          id: Date.now(),
          name: this.name,
          phone: this.phone,
          parent: this.parent || null,
        });
        this.name = "";
        this.phone = "";
        this.parent = "";
        this.isSubmitting = false;
      }, 1000); // Имитация задержки отправки
    },
  },
};
</script>

<style scoped>
.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input,
select {
  width: 100%;
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.error-message {
  color: red;
  font-size: 12px;
  margin-top: 5px;
}

.submit-button {
  padding: 10px 20px;
  font-weight: 600;
  color: white;
  font-size: 16px;
  border-radius: 4px;
  border: 1px solid #ccc;
  background-color: #00aeff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.submit-button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.submit-button:hover:not(:disabled) {
  background-color: #0051ff;
}
</style>
