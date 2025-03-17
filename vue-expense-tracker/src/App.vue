/* eslint-disable */
<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @submittedTransaction="handleTransactionSubmitted"/>
  </div>
</template>
<script lang = 'ts', setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import{useToast} from 'vue-toastification';
import { ref, computed, onMounted } from 'vue';

const toast = useToast();

const transactions = ref([]);

//Check Local Storage
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if(savedTransactions){
    transactions.value = savedTransactions;
  }

})

console.log(transactions.value); // Want to check value of each transaction

//Total off all transaction amounts -- Get Total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) =>{
    return acc + transaction.amount;
  }, 0);
});

// Get Income
const income = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) =>{
    return acc + transaction.amount;
  }, 0).toFixed(2);
});

// Get Expenses
const expenses = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) =>{
    return acc + transaction.amount;
  }, 0).toFixed(2);
});

//Add Transactions

const handleTransactionSubmitted = (transactionData) => {
  console.log(transactionData); // Log the field data
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  console.log(generateUniqueId());
  saveDataToLocalStorage();
  toast.success('Transaction Added');
};

//Unique Id for each transaction

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
}

//Delete Transaction

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) =>
  transaction.id !== id);
  
  saveDataToLocalStorage();
  toast.success('Transaction deleted');

  console.log(id);

};

//Save to Local Storage
const saveDataToLocalStorage = () =>{
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}


</script>

<style>

</style>
