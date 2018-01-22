<template>
  <div>
    <h1>{{getTeamScore(team)}}</h1>
    <p
      contenteditable="true"
      @input="updateName"
      @keydown.enter.prevent="updateName"
    >
      {{getTeamName(team)}}
    </p>
  </div>
</div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'score',
  props: {
    team: {
      required: true,
      type: String
    },
  },
  data () {
    return {
      caretPos: 0
    }
  },
  computed: {
    ...mapGetters([
      'getTeamName', 'getTeamScore'
    ])
  },
  updated () {
    let sel = window.getSelection();
    sel.collapse(sel.anchorNode, this.caretPos);
  },
  methods:{
    updateName:function(event){
      let sel = window.getSelection();
      this.caretPos = sel.anchorOffset; 
      this.$store.commit(
        'CHANGE_TEAM_NAME', {
          team: this.team, 
          name:event.target.innerText
        }
      )
    }
  }  
}
</script>

<style scoped>
  h1 {
    margin:0;
  }
  p {
    margin: 0;
  }
</style>


