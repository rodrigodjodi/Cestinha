<template>
  <modal
    @close="close"
  >
    <div class="modal-header">
      <h3>Novo Jogador</h3>
    </div>
    <div class="modal-body">
      <label for="playerName">Nome*</label>
      <input 
        ref = "input"
        type="text"
        name="playerName" id="playerName"
        v-model="playerName"
      />
      <div>
        <input type="checkbox" id="moveToBench" name="moveToBench" v-model ="moveToBench" checked/>
        <label for="moveToBench">Mover para o banco</label>
      </div>
      <p :class="[msgError ? 'msgError' : 'msgSuccess' ]">{{msg}}</p>
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
          @click="createPlayer"
        >
            Criar
        </button>
    </div>
  </modal>
</template>

<script>
import Modal from './Modal'
import toId from "../assets/toId";
export default {
  components: {
    Modal
  },
  props: {

  },
  computed: {
    id () {
      return toId(this.playerName);
    }
  },
  data () {
    return {
      playerName: '',
      moveToBench: true,
      msg:'',
      msgError: false
    }
  },
  methods: {
    close () {
      this.$emit('close');
    },
    createPlayer () {
      let payload = {
        _id: this.id,
        name: this.playerName
      }
      this.$store.dispatch('createPlayer', payload)
      .then(() => {
        return this.$store.dispatch('getPlayers')
      })
      .then(response => {
        if(moveToBench) {
          let player = response.find(entry => {
            return entry._id === this.id
          })  
          this.$store.commit('ADD_PLAYERS_TO_BENCH', [player])
        }
        this.msg = this.playerName + " criado com sucesso!"
        this.playerName = '';
        this.$refs.input.focus();

      })
      .catch(err => {
        this.$refs.input.focus();
        console.error(err);
        this.msgError = true;
        if (err.status === 409) {
          this.msg = 'Erro: esse nome já existe.'
        } else if (err.status === 412){
           this.msg = 'Erro: não pode ser deixado em branco!.'
        } else {
           this.msg = 'Erro: não foi possível fazer a inclusão do jogador.'
        }
      })
    }
  }
}
</script>

<style>
  .msgError {
    color: red;
  }
  .msgSuccess {
    color: green;
  }
</style>
