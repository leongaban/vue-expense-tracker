<template>
  <Header/>
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpenses/>
    <TransactionList :transactions="transactions"/>
    <AddTransaction/>
  </div>
</template>

<script setup>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeExpenses from './components/IncomeExpenses.vue'
  import TransactionList from './components/TransactionList.vue'
  import AddTransaction from './components/AddTransaction.vue'

  import { ref, computed } from 'vue'

  const transactions = ref([
    { id: 1, text: 'Flower', amount: -20.99 },
    { id: 2, text: 'Salary', amount: 300.12 },
    { id: 3, text: 'Book', amount: -10.58 },
    { id: 4, text: 'Camera', amount: 150.01 }
  ])

  const total = computed(() => {
    return transactions.value.reduce((acc, item) => (acc += item.amount), 0).toFixed(2)
  })

// Options API
  // export default {
  //   components: {
  //     Header,
  //     Balance,
  //     IncomeExpenses,
  //     TransactionList,
  //     AddTransaction
  //   }
  // }
</script>
