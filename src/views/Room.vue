<template>
  <Game @gameover="gameover" v-if="state == GAME_STATES.GAME_ACTIVE" ref="game"/>
  <Lobby @startgame="startgame" v-if="state == GAME_STATES.LOBBY"/>
</template>

<script>
import { ref, nextTick } from 'vue';
import Game from '@/components/Game.vue'
import Lobby from '@/components/Lobby.vue'

const GAME_STATES = {
  GAME_ACTIVE: 1,
  LOBBY: 2,
}

export default {
  name: 'Room',
  setup() {
    const state = ref(GAME_STATES.LOBBY);
    // const game = ref(null);
    return {state, GAME_STATES}
  },
  methods: {
    gameover() {
      this.state = GAME_STATES.LOBBY
    },
    startgame() {
      this.state = GAME_STATES.GAME_ACTIVE
      nextTick(() => {
        // console.log(...this, ...this.$refs, ...this.game)
        this.$refs.game.reset();
      })
    }
  },
  components: {
    Game,
    Lobby,
  }

}
</script>

<style>

</style>

