<script setup>
import { ref, defineEmits } from "vue";

const title = ref("");
const amount = ref("");
const type = ref("INCOME");
const emit = defineEmits(["addTransaction"]);

const addTransaction = () => {
  if (!title.value) return;

  // Convert amount to a number
  const transactionAmount = parseFloat(amount.value);
  // Check for negative or zero values
  if (transactionAmount <= 0) {
    alert("No negative or Zeroes please!");
    return; // Stop execution if invalid input
  }

  emit("addTransaction", {
    id: Date.now(),
    title: title.value,
    amount: transactionAmount,
    type: type.value,
  });

  // Clear inputs
  title.value = "";
  amount.value = "";
  type.value = "INCOME";
};
</script>

<template>
  <div class="input-group mb-3">
    <input v-model="title" type="text" class="form-control m-2 rounded" placeholder="Title" />
    <input v-model="amount" type="number" class="form-control m-2 rounded" placeholder="Amount" />
    <select v-model="type" class="form-select m-2 rounded">
      <option value="INCOME">Income</option>
      <option value="EXPENSE">Expense</option>
    </select>
    <button @click="addTransaction" class="btn btn-primary m-3 rounded">Add it</button>
  </div>
</template>

<style scoped></style>
