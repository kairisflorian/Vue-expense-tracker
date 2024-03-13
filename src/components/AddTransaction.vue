<template>
    <h3>Ajouter une nouvelle transaction</h3>
    <form id="form">
        <div class="form-control">
            <label for="text">Texte</label>
            <input type="text" id="text" placeholder="Entrez un texte..." />
        </div>
        <div class="form-control">
            <label for="montant">
                Montant <br />
                (négatif - dépense, positif - revenu)
            </label>
            <input
                type="number"
                id="amount"
                placeholder="Entrez le montant..."
            />
        </div>
        <button class="btn">Ajouter la transaction</button>
    </form>
</template>
