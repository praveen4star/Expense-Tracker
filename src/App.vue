<script setup>
  import { ref, computed, onMounted , watch} from 'vue';
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpense.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue'; 
  

  const transactions = ref([]);

  /** Load data from local storage */
  onMounted(( ) => {
    const savedTransactions = localStorage.getItem('transactions');
    if(savedTransactions){
      transactions.value = JSON.parse(savedTransactions);
    }
  });
  // calculate balance
  const totalBalance = computed(() => transactions.value.reduce((acc, transactions) => acc + transactions.amount, 0))
  const income = computed(() => transactions.value.filter(transaction => transaction.amount > 0).reduce((acc, transaction) => acc + transaction.amount, 0))
  const expense = computed(() => transactions.value.filter(transaction => transaction.amount < 0).reduce((acc, transaction) => acc + transaction.amount, 0))
  
  const addTransaction = (transaction) => {
    transactions.value.push(transaction)
  }
  const deleteTransaction = (id) =>{
    transactions.value = transactions.value.filter(transaction => transaction.id !== id);

  }
  // save transactions to local storage
  const saveTransactionsIntoLocalStorage = () => {
    try {
      localStorage.setItem('transactions', JSON.stringify(transactions.value))
    } catch (error) {
      console.error('Failed to save transactions:', error)
    }
};
  
  watch(transactions, () => {
    saveTransactionsIntoLocalStorage()
  }, { deep : true })
</script>

<template>
  <Header />
  <div class="container">
    <Balance :totalBalance="totalBalance"/>
    <IncomeExpense :income="income" :expense="Math.abs(expense)"/>
    <TransactionList :transactions="transactions"  @deleteTransaction="deleteTransaction"/>
    <AddTransaction @addTransaction="addTransaction"/>
  </div>
</template>

<style scoped>

</style>
