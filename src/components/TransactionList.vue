<template>
    <h3>Historique</h3>
    <ul id="list" class="list">
        <li
            v-for="transaction in transactions"
            :key="transaction.id"
            :class="transaction.amount < 0 ? 'minus' : 'plus'"
        >
            {{ transaction.text }}
            <span>
                {{ transaction.amount }}
            </span>
            <button
                class="delete-btn"
                @click="deleteTransaction(transaction.id)"
            >
                x
            </button>
        </li>
    </ul>
</template>

<script setup>
const props = defineProps({
    transactions: {
        type: Array,
        required: true,
    },
});

const emits = defineEmits(["transactionDeleted"]);

const deleteTransaction = (id) => {
    emits("transactionDeleted", id);
};
</script>
