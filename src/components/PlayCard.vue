<template>
    <div class="card">
      <i class="bi bi-suit-diamond" v-if="card.suit == 'diamonds'"></i>
      <i class="bi bi-suit-heart" v-if="card.suit == 'hearts'"></i>
      <i class="bi bi-suit-club-fill" v-if="card.suit == 'clubs'"></i>
      <i class="bi bi-suit-spade-fill" v-if="card.suit == 'spades'"></i>
      <br />
      <span>&nbsp;{{ card.rank }}</span>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      card: {
        type: Object,
        required: true
      }
    }
  };
  </script>

<style scoped>
.card {
    display: inline-block;
    border: 1px solid #000;
    padding: 10px;
    margin: 10px;
    width: 40px;
}
</style>