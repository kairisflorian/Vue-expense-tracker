<template>
    <h3>Ajouter une nouvelle transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Texte</label>
            <input
                type="text"
                id="text"
                placeholder="Entrez un texte..."
                v-model="text"
            />
        </div>
        <div class="form-control">
            <label for="montant">
                Montant <br />
                (négatif - dépense, positif - revenu)
            </label>
            <input
                type="text"
                id="amount"
                placeholder="Entrez le montant..."
                v-model="amount"
            />
        </div>
        <button class="btn">Ajouter la transaction</button>
    </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");

const toast = useToast();

const onSubmit = () => {
    if (!text.value || !amount.value) {
        toast.error("Les deux champs doivent être remplis.");
        return;
    }
    console.log(text.value, amount.value);
    text.value = "";
    amount.value = "";
};
</script>
