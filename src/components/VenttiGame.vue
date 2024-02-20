<template>
    <div class="game">
        <VenttiDealer :dealer="dealer" />
        <div class="table">
            <div class="top-container">
                <VenttiBank :bank="bank" />
            </div>

            <div class="center-container" v-if="gameDone">
                <h2 v-if="win">You win!</h2>
                <h2 v-else>You lose!</h2>
                <button @click="restart">Play again</button>
            </div>

            <div class="bottom-container" id="betting" v-if="!started">
                Your bet:
                <button @click="increaseBet">+50</button>
                <span>&nbsp;{{ player.betAmount }}&nbsp;</span>
                <button @click="decreaseBet">-50</button>
                <button @click="bet" v-if="player.betAmount > 0">Bet</button>
            </div>
        </div>
        <VenttiPlayer :player="player" /> <br />
        <div v-if="playersTurn"> 
        <button @click="hit">Hit</button>
        <button @click="stand">Stand</button>
        </div>
    </div>
</template>

<script>
import VenttiPlayer from './VenttiPlayer.vue';
import VenttiDealer from './VenttiDealer.vue';
import VenttiBank from './VenttiBank.vue';
//import PlayCard from './PlayCard.vue';

export default {
    name: 'VenttiGame',
    components: {
    VenttiPlayer, VenttiDealer,
    VenttiBank
},
    data() {
        return {
            started: false,
            playersTurn: false,
            gameDone: false,
            win: false,

            bank: {
                balance: 0
            },

            dealer: {
                hand: []
            },
            
            player: { 
                name: 'Player 1', betAmount: 0, balance: 1000, hand: [] 
            },

            deck: {}
        };
    },
    methods: {
        initialDeal() {
            // Deal cards to players
            this.playersTurn = false;
            this.initDeck();
            this.player.hand.push(this.deck.pop());
            this.player.hand.push(this.deck.pop());
            this.checkScore();
            this.dealer.hand.push(this.deck.pop());
            this.dealer.hand.push(this.deck.pop());
            
            if (this.calculateScore(this.dealer.hand) > 21) {
                this.finish(true);
            } else if (this.calculateScore(this.dealer.hand) == 21) {
                this.finish(false);
            }
        },

        checkDealerScore() {
            let dealerScore = this.calculateScore(this.dealer.hand);
            if (dealerScore > 21) {
                this.finish(true);
            } else if (dealerScore > this.calculateScore(this.player.hand)) {
                this.finish(false);
            } else {
                this.finish(true);
            }
        },

        shuffle(array) {
            // Shuffle the array using the Fisher-Yates algorithm
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        },

        increaseBet() {
            this.player.betAmount += 50;
        },


        decreaseBet() {
            this.player.betAmount -= 50;
            if (this.player.betAmount < 0) {
                this.player.betAmount = 0;
            }
            
        },

        bet() {
            this.player.balance -= this.player.betAmount;
            this.bank.balance += this.player.betAmount;
            this.started = true;
            this.runGame();
        },

        runGame() {
            // Double bank from dealer
            this.bank.balance += this.player.betAmount;
            this.initialDeal();
            if (!this.gameDone) {
                this.playersTurn = true;
            }
        }, 

        hit() {
            this.player.hand.push(this.deck.pop());
            this.checkScore();
        },

        stand() {
            this.playersTurn = false;
            this.dealerTurn();
        },

        calculateScore(hand) {
            let score = 0;
            for (let i = 0; i < hand.length; i++) {
                if (hand[i].rank === 'J') {
                    score += 11;
                } else if (hand[i].rank === 'Q') {
                    score += 12;
                } else if (hand[i].rank === 'K') {
                    score += 13;
                } else if (hand[i].rank === 'A') {
                    score += 1;
                } else {
                    score += parseInt(hand[i].rank);
                }
            }
            return score;
        },

        checkScore() {
            let playerScore = this.calculateScore(this.player.hand);
            if (playerScore > 21) {
                this.finish(false);
            }
        },

        dealerTurn() {
            let dealerScore = this.calculateScore(this.dealer.hand);
            while (dealerScore < 17) {
                this.dealer.hand.push(this.deck.pop());
                dealerScore = this.calculateScore(this.dealer.hand);
            }
            this.checkDealerScore();
        },

        finish(won) {
            this.gameDone = true;
            this.win = won;
            this.playersTurn = false;
            
            if (won) {
                this.player.balance += this.bank.balance;
            }
            this.bank.balance = 0;
        },

        restart() {
            this.player.hand = [];
            this.dealer.hand = [];
            this.player.betAmount = 0;
            this.gameDone = false;
            this.started = false;
            this.playersTurn = false;
            this.win = false;
        }, 
        initDeck() {
            this.deck = [
                { id: 1, rank: 'A', suit: 'hearts' },
                { id: 2, rank: '2', suit: 'hearts' },
                { id: 3, rank: '3', suit: 'hearts' },
                { id: 4, rank: '4', suit: 'hearts' },
                { id: 5, rank: '5', suit: 'hearts' },
                { id: 6, rank: '6', suit: 'hearts' },
                { id: 7, rank: '7', suit: 'hearts' },
                { id: 8, rank: '8', suit: 'hearts' },
                { id: 9, rank: '9', suit: 'hearts' },
                { id: 10, rank: '10', suit: 'hearts' },
                { id: 11, rank: 'J', suit: 'hearts' },
                { id: 12, rank: 'Q', suit: 'hearts' },
                { id: 13, rank: 'K', suit: 'hearts' },
                { id: 14, rank: 'A', suit: 'diamonds' },
                { id: 15, rank: '2', suit: 'diamonds' },
                { id: 16, rank: '3', suit: 'diamonds' },
                { id: 17, rank: '4', suit: 'diamonds' },
                { id: 18, rank: '5', suit: 'diamonds' },
                { id: 19, rank: '6', suit: 'diamonds' },
                { id: 20, rank: '7', suit: 'diamonds' },
                { id: 21, rank: '8', suit: 'diamonds' },
                { id: 22, rank: '9', suit: 'diamonds' },
                { id: 23, rank: '10', suit: 'diamonds' },
                { id: 24, rank: 'J', suit: 'diamonds' },
                { id: 25, rank: 'Q', suit: 'diamonds' },
                { id: 26, rank: 'K', suit: 'diamonds' },
                { id: 27, rank: 'A', suit: 'clubs' },
                { id: 28, rank: '2', suit: 'clubs' },
                { id: 29, rank: '3', suit: 'clubs' },
                { id: 30, rank: '4', suit: 'clubs' },
                { id: 31, rank: '5', suit: 'clubs' },
                { id: 32, rank: '6', suit: 'clubs' },
                { id: 33, rank: '7', suit: 'clubs' },
                { id: 34, rank: '8', suit: 'clubs' },
                { id: 35, rank: '9', suit: 'clubs' },
                { id: 36, rank: '10', suit: 'clubs' },
                { id: 37, rank: 'J', suit: 'clubs' },
                { id: 38, rank: 'Q', suit: 'clubs' },
                { id: 39, rank: 'K', suit: 'clubs' },
                { id: 40, rank: 'A', suit: 'spades' },
                { id: 41, rank: '2', suit: 'spades' },
                { id: 42, rank: '3', suit: 'spades' },
                { id: 43, rank: '4', suit: 'spades' },
                { id: 44, rank: '5', suit: 'spades' },
                { id: 45, rank: '6', suit: 'spades' },
                { id: 46, rank: '7', suit: 'spades' },
                { id: 47, rank: '8', suit: 'spades' },
                { id: 48, rank: '9', suit: 'spades' },
                { id: 49, rank: '10', suit: 'spades' },
                { id: 50, rank: 'J', suit: 'spades' },
                { id: 51, rank: 'Q', suit: 'spades' },
                { id: 52, rank: 'K', suit: 'spades' }
            ];
            this.shuffle(this.deck);
        }
    }
};
</script>

<style scoped>
.game {
    border: 0px solid #000;
    padding: 10px;
    margin: 10px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.table {
    border: 1px solid #000;
    padding: 10px;
    margin: 10px;
    display: grid;
    grid-template-rows: auto 1fr auto;
    justify-content: center;
    align-items: center;
    width: 300px;
    height: 180px;
}

.top-container {
    grid-row: 1;
    display: flex;
    justify-content: center;
    align-items: center;
}

.center-container {
    grid-row: 2;
    display: inline-block;
    justify-content: center;
    align-items: center;
}

.bottom-container {
    grid-row: 3;
    display: flex;
    justify-content: center;
    align-items: center;
}

</style>