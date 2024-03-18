<template>
    <Header />
    <div class="container">
        <Balance :total="+total" />
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <TransactionList
            :transactions="transactions"
            @transactionDeleted="handleTransactionDeleted"
        />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const transactions = ref([]);

const toast = useToast();

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
});

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
    saveTransactionsToLocalStorage();
    toast.success("Transaction ajoutée !");
};

// Générateur d'ID unique
const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
};

// Supprimer une transaction
const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => {
        transaction.id !== id;
    });
    saveTransactionsToLocalStorage();
    toast.success("La transaction a été supprimée !");
};

// Sauvegarder dans le localstorage
const saveTransactionsToLocalStorage = () => {
    localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
