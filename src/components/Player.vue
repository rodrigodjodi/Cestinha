<template>
  <div class="player" @click="$emit('select', payload)">
    {{playerObj.name}}
  </div>
</template>

<script>
export default {
  name: 'player',
  props: {
    playerObj: {
      type: Object,
      required: true
    },
    team: {
      type: String
    }
  },
  computed: {
    payload () {
      let temp = this.playerObj;
      temp.team = this.team
      return temp
    }
  }
}
</script>

<style>
  .player {
  background: #393;
  display:block;
  height: 48px;
  margin: 4px;

}
</style>
