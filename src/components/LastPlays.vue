<template>
  <ul class="history-list">
    <li class="history-item" v-for="entry in plays"
      :key="entry.ts"
    > 
      <div>
      {{entry.player}} fez {{entry.actionText}}
        <button @click="deletePlay(entry._id)">X</button>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  computed:{
    plays () {
      return this.$store.state.game.plays
    }
  },
  methods: {
    deletePlay(id) {
      this.$store.dispatch('deletePlay', id);
    }
  }
}
</script>

<style>
li.history-item{
  list-style-type: none;
  height: 48px;
  background-color: #FFF;
  border-bottom: 1px solid #ccc;
  padding: 8px;
  
}
ul.history-list{
padding: 0;
}
</style>
