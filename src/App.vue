<template>
  <Header/>
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
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

  import { ref, computed, onMounted } from 'vue'

  const toast = useToast()

  const transactions = ref([])

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

    if (savedTransactions) {
      transactions.value = savedTransactions
    }
  })

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
      .reduce((acc, item) => (acc += item.amount), 0)
      .toFixed(2)
  })

  // Generate unique ID
  const generateUniqueId = () => Math.random().toString(36).substr(2, 9)

  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    })

    saveToLocalStorage()

    toast.success('Transaction added!')
  }

  // Delete transaction
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(transaction => transaction.id !== id)
    saveToLocalStorage()
    toast.success('Transaction deleted!')
  }

  // Save to local storage
  const saveToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }
</script>
