<template>
    <div class="column is-one-third has-text-centered">
        <button @click="toggleCounter" :disabled="!ready">{{timeLeft | convert}}</button>
    </div>
</template>


<script>
export default {
    name : 'clock',
    computed :{
      timeLeft () {
        return this.$store.state.game.timeLeft;
      },
      gameOn () {
        return this.$store.state.game.gameOn;
      },
      gameDuration () {
        return this.$store.state.settings.gameDuration;
      },
      ready () { //controls disabled state of the clock
      //will only be true if there is at least 3 players in each team
        if ( this.$store.state.game.team1.players.length > 2 &&
          this.$store.state.game.team2.players.length > 2)
        {
          return true;

        } else {
          return false;
        }
      }
    },
    data () {
        return {
        }
    },
    methods: {
        toggleCounter () {
            if(this.gameOn) {
              this.$store.dispatch('clockStop');
            } else {
              if(this.timeLeft === 0) {
                this.$store.dispatch('clockReset');
              } else {
                this.$store.dispatch('clockStart');
              }
            }
        }
    },
    mounted () {
      this.$store.dispatch('clockReset');
    },
    filters : {
        convert (value) {
            var seconds = value % 60;
            if (seconds.toString().length <=1){
                seconds = '0' + seconds.toString();
            }
            var minutes = Math.trunc(value /60) % 60;
            return minutes + ':' + seconds;
        }
    }
}
</script>
<style scoped>
button {
  font-size: 2em;
  margin-top: 12px;
}
</style>
