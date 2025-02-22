<script setup>
import { ref, computed, onMounted } from "vue";
import AddTransaction from "./components/AddTransaction.vue";
import TransactionList from "./components/TransactionList.vue";

const transactions = ref([]);
const filter = ref("ALL");


const addTransaction = (transaction) => {
  transactions.value.unshift(transaction);
  saveToLocalStorage();
};


const saveToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};


onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});
// Filtering transactions
const filteredTransactions = computed(() => {
  if (filter.value == "All") return transactions.value;
  let x = transactions.value.filter((t) => t.type == filter.value);
  return x;
});

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((t) => t.id !== id);
  saveToLocalStorage();
};
</script>

<template>
  <div class="container mt-5 p-4">
    <h2 style="text-align: center" class="h1 mb-5">Income & Expense Tracker</h2>

    <!-- Add Transaction -->
    <AddTransaction @addTransaction="addTransaction" />


    <div class="mb-5">
      <div class="form-label text-light" :v-model="filter">
        <input type="radio" id="all" value="All" v-model="filter" />
        <label for="all" class="text-dark m-2">All</label>

        <input type="radio" id="INCOME" value="INCOME" v-model="filter" />
        <label for="INCOME" class="text-dark m-2">Income</label>
        <input type="radio" id="EXPENSE" value="EXPENSE" v-model="filter" />
        <label for="EXPENSE" class="text-dark m-2">Expense</label>
      </div>
    </div>

    
    <TransactionList :transactions="filteredTransactions" @deleteTransaction="deleteTransaction" />
  </div>
</template>

<style scoped></style>
