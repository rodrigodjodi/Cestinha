<template>
  <modal
    @close="close"
  >
    <div class="modal-header">
      <h3>{{actingPlayer.name}} fez...</h3>
    </div>
    <div class="modal-body">
      <actionButton
        v-for="button in actionButtons"
        :key="button.text"
        :btnData="button"
        :player="actingPlayer"
        @close="close"
      />
      </div>
      <div class="modal-footer text-right">
          <button
            class="modal-default-button"
            @click="close"
          >
              Fechar
          </button>
      </div>
  </modal>
</template>

<script>
import Modal from './Modal'
import ActionButton from './ActionButton'
export default {
  components: {
    Modal, ActionButton
  },
  props: {
    show: {
      type: Boolean,
      default: false
    },
    actingPlayer: {
      type: Object,
      required: true
    }
  },
  computed: {
    actionButtons () {
      return this.$store.state.settings.actionButtons
    },
  },
  data () {
    return {
      
    }
  },
  methods: {
    close () {
      this.$emit('close');
    }
  }
}
</script>

<style>

</style>
