<template>
  <modal
    @close="close"
  >
    <div class="modal-header">
      <h3>Selecionar jogadores</h3>
    </div>
    <div class="modal-body">
      <player-select
        :options="players"
        @change="fillSelected"
  
      />
    </div>
    <div class="modal-footer text-right">
        <button
          class="modal-default-button"
          @click="close"
        >
            Fechar
        </button>
        <button
          class="modal-default-button"
          @click="selectPlayers"
        >
            Selecionar
        </button>
    </div>
  </modal>
</template>

<script>
import Modal from './Modal'
import PlayerSelect from './PlayerSelect'
export default {
  components: {
    Modal, PlayerSelect
  },
  computed: {
    players () {
      //allplayers minus allocated players
      let allocated = this.$store.state.game.bench.players.concat(
        this.$store.state.game.team1.players, this.$store.state.game.team2.players
      )
      let ids = allocated.map(player => player._id)
      let filtered = this.$store.state.settings.allPlayers.filter(player => {
        return !ids.includes(player._id)
      });
      return filtered;
      
    }
  },
  data () {
    return {
      selected: []
    }
  },
  methods: {
    close () {
      this.$emit('close');
    },
    toggleSelected (playerObj) {
      let index = this.selected.findIndex(function (obj) { return obj._id === playerObj._id; });
      console.log(index);
      if (index === -1) {
        this.selected.push(playerObj)
      } else {
        this.selected.splice(index, 1)
      }
    },
    fillSelected (payload) {
      this.selected = payload;
    },
    selectPlayers () {
      let benchPlayers = this.selected.map((value) => {
        let playerObj = this.players.filter(function(player){
          return player._id === value
        })
        return playerObj[0]
      })
      //console.log(benchPlayers);
      this.$store.commit('ADD_PLAYERS_TO_BENCH', benchPlayers)
      this.close();
    }
  }
}
</script>

<style>

</style>

