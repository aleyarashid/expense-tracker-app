<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps(["transactions"]);
const emit = defineEmits(["deleteTransaction"]);

const deleteTransaction = (id) => {
  emit("deleteTransaction", id);
};
</script>

<template>
  <table class="table table-dark">
    <thead>
      <tr>
        <th>Title</th>
        <th>Amount</th>
        <th>Type</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr v-if="transactions.length === 0">
        <td colspan="4" class="text-center">No transactions recorded yet.</td>
      </tr>

      <tr v-for="transaction in transactions" :key="transaction.id">
        <td>{{ transaction.title }}</td>

        <td
          v-bind:class="{
            'text-danger': transaction.type === 'EXPENSE',
            'text-danger, fw-bolder': transaction.type === 'EXPENSE' && transaction.amount > 499,
            'text-success': transaction.type === 'INCOME',
          }"
        >
          ${{ transaction.amount }}
        </td>
        <td>{{ transaction.type }}</td>
        <td>
          <button @click="deleteTransaction(transaction.id)" class="btn btn-danger">Delete</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped></style>
