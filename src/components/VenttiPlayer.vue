<template>
    <div class="player">
        <h2>{{  player.name }}</h2>
        <p>Balance: {{ player.balance }}</p>

        <div class="card-container">
            <div v-for="card in player.hand" :key="card.id">
                <PlayCard :card="card" />
            </div>
        </div>
    </div>
</template>

<script>
import PlayCard from './PlayCard.vue';
import VenttiBank from './VenttiBank.vue';

export default {
    name: 'VenttiPlayer',
    props: {
        player: { 
            type: Object,
            required: true
        }
    },
    components: {
        PlayCard
    },
    methods: {
        bet() {
            this.balance -= this.bet;
            VenttiBank.balance += this.bet;
        },


        hit() {
            
        },

        stand() {
        }
    }
}
</script>

<style scoped>
.player {
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