<template>
  <button @click="showModal = true">Agregar movimiento</button>
  <teleport to="#app">
    <ModalPage v-show="showModal" @close="showModal = false">
      <form @submit.prevent="submit">
        <div class="field">
          <label for="">Título</label>
          <input type="text" v-model="title" />
        </div>
        <div class="field">
          <label for="">Monto</label>
          <input type="number" v-model="amount" />
        </div>
        <div class="field">
          <label for="">Descripción</label>
          <textarea rows="4" v-model="description"></textarea>
        </div>
        <div class="field">
          <label for="">Tipo de movimiento</label>
          <label class="radio-label">
            <input type="radio" v-model="movementType" value="Ingreso" />
            <span>Ingreso</span>
          </label>
          <label class="radio-label">
            <input type="radio" v-model="movementType" value="Gasto" />
            <span>Gasto</span>
          </label>
        </div>
        <div class="action">
          <button>Agregar movimiento</button>
        </div>
      </form>
    </ModalPage>
  </teleport>
</template>

<script setup>
import ModalPage from "./ModalPage.vue";
import { ref, defineEmits } from "vue";

const showModal = ref(false);
const title = ref("");
const amount = ref(0);
const description = ref("");
const movementType = ref("Ingreso");

const emit = defineEmits(["create"]);

const submit = () => {
  showModal.value = !showModal.value;
  emit("create", {
    title: title.value,
    description: description.value,
    amount: movementType.value === "Ingreso" ? amount.value : -amount.value,
    time: new Date(),
    id: new Date().getTime(),
  });
  title.value = "";
  description.value = "";
  amount.value = 0;
  movementType.value = "Ingreso";
};
</script>

<style scoped>
button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--brand-blue);
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
}

form {
  font-size: 1.24rem;
  width: 50vw;
  margin: 0 auto;
}

form .action {
  padding: 0 24px;
}

.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}

label {
  margin-bottom: 8px;
}

input,
textarea {
  font-size: 1.24rem;
  border: 2px solid var(--brand-blue);
  border-radius: 8px;
  padding: 8px;
}

input[type="number"] {
  text-align: right;
}

.radio-label {
  display: flex;
  align-items: center;
  margin-top: 8px;
}

.radio-label span {
  margin-top: 4px;
  margin-left: 8px;
}

input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: var(--brand-blue);
  border: 2px solid var(--brand-blue);
  border-radius: 50%;
}

input[type="radio"]:checked {
  background-color: var(--brand-blue);
}
@media (max-width: 700px) {
  form {
    width: 100vw;
  }
}
</style>
