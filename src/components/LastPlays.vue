<template>
  <ul>
    <li v-for="entry in plays"
      :key="entry.ts"
    > 
     {{entry.player}} fez {{entry.actionText}}
    </li>
  </ul>
</template>

<script>
export default {
  computed:{
    plays () {
      return this.$store.state.game.plays
    }
  }
}
</script>

<style>

</style>
