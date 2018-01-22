<template>
  <div>
    <div class="container">
      <score
        team="team1"
        class="score1"
      />
      <score 
        team="team2"
        class="score2"
      />
      <div class="clock">
        <clock/>
      </div>
      
      <div class="team1">
        <draggable class="teamcontainer"
          v-model="team1Players"
          
          :options="dragOptions"
        >
          <player 
            @select="onPlayerClicked"
            v-for="player in team1Players"
            :playerObj="player"
            :key="player._id"
            team = "team1"
          />
        </draggable>
      </div>

      <div class="team2">
        <draggable class="teamcontainer"
          v-model="team2Players"
          
          :options="dragOptions"
        >
          <player 
            @select="onPlayerClicked"
            v-for="player in team2Players"
            :playerObj="player"
            :key="player._id"
            team = "team2"
          />
        </draggable>
      </div>

      <div class="context">
        <last-plays v-if="gameOn"/>

        <template v-else="gameOn">
          <button
            @click="SelectPlayersModalVisible = true"
          >
          Adicionar jogadores ao banco
          </button>
          <button
            @click="CreatePlayerModalVisible = true"
          >
          Cadastrar jogador
          </button>
          <draggable class="teamcontainer"
            v-model="bench"
            :options="dragOptions"
          >
            <player 
              v-for="player in bench"
              :playerObj="player"
              :key="player._id"
            />
          </draggable>
        </template>
      </div>
    </div>

    <actionModal
      v-if="actionModalVisible"
      :actingPlayer="actingPlayer"
      @close="actionModalVisible = false"
    />
    <CreatePlayerModal
      v-if="CreatePlayerModalVisible"
      @close = "CreatePlayerModalVisible = false"
    />
    <SelectPlayersModal
      v-if="SelectPlayersModalVisible"
      @close = "SelectPlayersModalVisible = false"
    />
  </div>
</template>

<script>
import Score from './Score'
import Draggable from 'vuedraggable'
import Player from './Player'
import Clock from './Clock'
const ActionModal = () => import('./ActionModal')
const CreatePlayerModal = () => import('./CreatePlayerModal')
const SelectPlayersModal = () => import('./SelectPlayersModal')
import LastPlays from './LastPlays'
import { mapGetters } from 'vuex'

export default {
  name: 'basegrid',
  components: {
    Score,
    Draggable,
    Player,
    Clock,
    ActionModal,
    LastPlays,
    CreatePlayerModal,
    SelectPlayersModal
  },
  data () {
    return {
      actingPlayer: "",
      actionModalVisible : false,
      CreatePlayerModalVisible: false,
      SelectPlayersModalVisible: false,
      dragOptions: {
        group:'players',
        fallbackTolerance : 0,
        delay:0
      }
    }
  },
  computed: {
    gameOn () {
      return this.$store.state.game.gameOn
    },
    team1Players: {
      get () {
        return this.$store.state.game.team1.players
      },
      set (value) {
        this.$store.commit('UPDATE_LIST',{team:"team1", value: value})
      },
    },
    team2Players: {
      get () {
        return this.$store.state.game.team2.players
      },
      set (value) {
        this.$store.commit('UPDATE_LIST',{team:"team2", value: value})
      },
    },
    bench: {
      get () {
        return this.$store.state.game.bench.players
      },
      set (value) {
        this.$store.commit('UPDATE_LIST',{team:"bench", value: value})
      },
    },

  },
  methods: {
    onPlayerClicked (payload) {
      if (this.gameOn) {
        this.actionModalVisible = true;
        this.actingPlayer = payload
      } else {
        console.log(payload.team + ' clicado.');
      }
    },
  },
  created () {
    this.$store.dispatch('getPlayers');
  }
}
</script>

<style>
.container {
  height: 100%;
  display: grid;
  grid-gap: 0;
  grid-template-columns: repeat(6, 1fr);
  grid-template-rows: 72px 320px auto;
  grid-template-areas: 
        "s1 s1 c c s2 s2"
        "t1 t1 t1 t2 t2 t2"
        "ct ct ct ct ct ct";
}

html, body {
  box-sizing: border-box;
  background-color: #ccc;
  margin: 0px;
  height: 100%;
}
.score1 {
    grid-area: s1;
    text-align: center;
    background-color: #aaa;
    padding:4px;
    margin: 0 2px;
  }
  .score2 {
    grid-area: s2;
    text-align: center;
    background-color: #aaa;
    padding:4px;
    margin: 0 2px;
  }
  .clock {
    grid-area: c;
  }
  .team1 {
    grid-area: t1;
  }
  .team2 {
    grid-area: t2;
  }
  .teamcontainer{
    height: 100%;
    background-color: #aaa;
    margin: 0 2px;
    padding: 2px;
  }
  .context {
    grid-area: ct;
    overflow:auto;
    min-height: 50px;
  }
</style>
