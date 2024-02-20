<template>
    <div class="dealer">
        <h2>Dealer</h2>
        <div class="card-container">
            <div v-for="card in dealer.hand" :key="card.id">
                <PlayCard :card="card" />
            </div>
        </div> 
    </div>
</template>

<script>
import PlayCard from './PlayCard.vue';

export default {
    name: 'VenttiDealer',
    props: {
        dealer: { 
            type: Object,
            required: true
        }
    },
    components: {
        PlayCard
    }
}
</script>

<style scoped>
.dealer {
    border: 1px solid #000;
    padding: 10px;
    margin: 10px;
    display: inline-block;
    align-self: center;
}

.card-container {
    display: flex;
    flex-wrap: wrap;
}
</style>