<template>
  <Header/>
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<script setup>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeExpenses from './components/IncomeExpenses.vue'
  import TransactionList from './components/TransactionList.vue'
  import AddTransaction from './components/AddTransaction.vue'

  import { useToast } from 'vue-toastification'

  import { ref, computed } from 'vue'

  const toast = useToast()

  const transactions = ref([
    { id: 1, text: 'Flower', amount: -20.99 },
    { id: 2, text: 'Salary', amount: 300.12 },
    { id: 3, text: 'Book', amount: -10.58 },
    { id: 4, text: 'Camera', amount: 150.01 }
  ])

  // Get Total
  const total = computed(() => {
    return transactions.value.reduce((acc, item) => (acc += item.amount), 0).toFixed(2)
  })

  // Get income
  const income = computed(() => {
    return transactions.value
      .filter(item => item.amount > 0)
      .reduce((acc, item) => (acc += item.amount), 0)
      .toFixed(2)
  })

  // Get expenses
  const expenses = computed(() => {
    return transactions.value
      .filter(item => item.amount < 0)
      .reduce((acc, item) => (acc += item.amount), 0) * -1
      .toFixed(2)
  })

  // Generate unique ID
  const generateUniqueId = () => {
    return Math.random().toString(36).substr(2, 9)
  }

  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    })

    toast('Transaction added!', {
      type: 'success'
    })
  }
</script>
