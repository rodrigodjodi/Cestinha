<template>
  <div class="actionButton" @click.stop="dispatchAction">
    {{btnData.actionText}}
  </div>
</template>

<script>
export default {
  props: {
    //btnData: {action: String action, actionText: String text}
    btnData: {
      type: Object,
      required: true
    },
    player: {
      required: true
    }
    
  },
  methods: {
    //the button is responsable for dispatching the action
    dispatchAction () {
      let payload = {
        _id: new Date().toISOString(),
        timeLeft: this.$store.state.game.timeLeft,
        player: this.player._id,
        playerName: this.player.name,
        action: this.btnData.action,
        actionText: this.btnData.actionText,
        team: this.player.team,
        game: this.$store.state.game.name
      }
      //console.log(payload);
      this.$store.dispatch('addPlay', payload);
      this.$emit('close')
    }
  },
}
</script>

<style>
  .actionButton {
    height: 48px;
    width: 100%;
    position: relative;
    background-color: #697;
    margin:4px
  }
</style>
