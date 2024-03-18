<template>
    <Header />
    <div class="container">
        <Balance :total="+total" />
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <TransactionList :transactions="transactions" />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed } from "vue";
import { useToast } from "vue-toastification";

const transactions = ref([
    { id: 1, text: "Fleurs", amount: -19.99 },
    { id: 2, text: "Salaire", amount: 299.99 },
    { id: 3, text: "Livre", amount: -10 },
    { id: 4, text: "Caméra", amount: 150 },
]);

const toast = useToast();

// Récupérer le total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

// Récupérer le revenu
const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0)
        .toFixed(2);
});

// Récupérer les dépenses
const expenses = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0)
        .toFixed(2);
});

// Ajout de la transaction
const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount,
    });
    toast.success("Transaction ajoutée !");
};

// Générateur d'ID unique
const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
};
</script>
