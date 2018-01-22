<template>
  <div>
    <div v-for="player in options" :key="player._id">
      <input 
        type="checkbox"
        :id="player._id"
        :value="player._id"
        v-model="selected"
        @change="$emit('change', selected)"
      />
      <label :for="player._id">{{player.name}}</label>
    </div>
  </div>
</template>

<script>
export default {
  props: ['options'],
  data () {
    return {
      selected: []
    }
  }

}
</script>

<style>

</style>
